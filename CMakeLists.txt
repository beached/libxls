cmake_minimum_required(VERSION 3.16)
project(xls
        VERSION "1.6.2"
        DESCRIPTION "Read binary Excel files from C/C++"
        HOMEPAGE_URL "https://github.com/beached/libxls"
        LANGUAGES C CXX
        )

find_package( Iconv )

set(SOURCE_FILES
        src/xlstool.c
        src/endian.c
        src/locale.c
        src/ole.c
        src/xls.c
        )

include(GNUInstallDirs)

option(BUILD_SHARED_LIBS "Build shared library" ON)
add_library(xlsreader ${SOURCE_FILES} )
add_library( ${PROJECT_NAME}::xlsreader ALIAS xlsreader )

if( Iconv_FOUND )
	target_link_libraries( xlsreader PRIVATE Iconv::Iconv )
endif( )

target_compile_definitions(xlsreader PRIVATE PACKAGE_VERSION="${PROJECT_VERSION}" HAVE_XLOCALE_H=1)

target_include_directories( xlsreader PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
        PRIVATE src)

set_target_properties(xlsreader PROPERTIES
        VERSION ${PROJECT_VERSION}
        SOVERSION 1)

add_subdirectory( cplusplus/ )

install(TARGETS xlsreader xlsreadercpp EXPORT LibXlsConfig)
install(DIRECTORY include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
install(DIRECTORY cplusplus/include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

set(libxls_INSTALL_CMAKEDIR "share/LibXls/cmake"
    CACHE STRING "Install location for generated CMake files.")

install(EXPORT LibXlsConfig DESTINATION ${libxls_INSTALL_CMAKEDIR})
export(TARGETS xlsreader xlsreadercpp FILE LibXlsConfig.cmake)
