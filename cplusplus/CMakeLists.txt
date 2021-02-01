
add_library( xlsreadercpp src/XlsReader.cpp )
target_link_libraries( xlsreadercpp PRIVATE xlsreader )
add_library( ${PROJECT_NAME}::xlsreadercpp ALIAS xlsreadercpp )

target_include_directories( xlsreadercpp PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
        PRIVATE src)

set_target_properties(xlsreadercpp PROPERTIES
        VERSION ${PROJECT_VERSION}
        SOVERSION 1)

