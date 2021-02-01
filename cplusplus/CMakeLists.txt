
set(CMAKE_CXX_STANDARD 17 CACHE STRING "The C++ standard whose features are requested.")
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

add_library( xlsreadercpp src/XlsReader.cpp )
target_link_libraries( xlsreadercpp PUBLIC xlsreader )

add_library( ${PROJECT_NAME}::xlsreadercpp ALIAS xlsreadercpp )

target_include_directories( xlsreadercpp PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
        PRIVATE src)

set_target_properties(xlsreadercpp PROPERTIES
        VERSION ${PROJECT_VERSION}
        SOVERSION 1)

