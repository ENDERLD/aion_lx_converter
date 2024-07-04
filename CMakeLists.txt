cmake_minimum_required(VERSION 3.10)
project(cpp_template)
message(STATUS "Project Name: ${PROJECT_NAME}")

include(autonomous_cmake/autonomous.cmake)
add_subdirectory(autonomous_utilities)
add_subdirectory(autonomous_proto)
add_subdirectory(mw_api)

add_executable(cpp_template main.cpp)
autonomous_set_target(cpp_template)
autonomous_print_target_info(cpp_template)
install(TARGETS cpp_template
        DESTINATION bin
)


message(STATUS "System Processor: ${CMAKE_SYSTEM_PROCESSOR}")
execute_process(
        COMMAND lsb_release -sc
        OUTPUT_VARIABLE CODENAME
        OUTPUT_STRIP_TRAILING_WHITESPACE
)
message(STATUS "Codename: ${CODENAME}")
string(TIMESTAMP CURRENT_TIME "%Y%m%d_%H%M%S")
message(STATUS "CMake command executed at: ${CURRENT_TIME}")

set(CPACK_GENERATOR "DEB")
set(CPACK_DEBIAN_PACKAGE_MAINTAINER "${CURRENT_TIME} <todo@todo.com>")
set(CPACK_PACKAGE_FILE_NAME ${PROJECT_NAME}_${CODENAME}_${CMAKE_SYSTEM_PROCESSOR})
set(CPACK_OUTPUT_FILE_PREFIX ${CMAKE_CURRENT_SOURCE_DIR})
set(CPACK_DEB_COMPONENT_INSTALL ON)
set(CPACK_COMPONENTS_ALL app)

include(CPack)
