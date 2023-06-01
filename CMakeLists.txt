cmake_minimum_required(VERSION 3.22)
find_program(CCACHE_PROGRAM ccache)

project(zlib-test)

if(CCACHE_PROGRAM)
    MESSAGE("CCACHE support enabled")
    set_property(GLOBAL PROPERTY RULE_LAUNCH_COMPILE "${CCACHE_PROGRAM}")
    set_property(GLOBAL PROPERTY RULE_LAUNCH_LINK "${CCACHE_PROGRAM}")
endif()

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
#set(CMAKE_CXX_COMPILER /usr/bin/clang++)

set(CMAKE_BUILD_TYPE "Release")

set(FETCHCONTENT_QUIET FALSE)
include(FetchContent)

set(BUILD_SHARED_LIBS OFF)
set(BUILD_STATIC_LIBS ON)
set(BUILD_TESTING OFF)

FetchContent_Declare(
    fmt
    GIT_REPOSITORY "https://github.com/fmtlib/fmt"
    GIT_TAG 9.1.0
    GIT_PROGRESS TRUE
    GIT_SHALLOW TRUE
)

set(ZLIB_COMPAT ON)

FetchContent_Declare(
    zlib
    GIT_REPOSITORY "https://github.com/zlib-ng/zlib-ng"
    GIT_TAG 2.0.6
    GIT_PROGRESS TRUE
    GIT_SHALLOW TRUE
)

FetchContent_MakeAvailable(fmt zlib)

#add_compile_options(-Wall -Wextra -pedantic -Werror)

add_executable(test1 src/EntryPoint.cpp)

target_include_directories(test1 PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/src/)

#if(CMAKE_BUILD_TYPE STREQUAL "Release")
#
#    target_include_directories(test1 PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/vendor/glad/release/include/)
#    target_sources(test1 PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/vendor/glad/release/src/glad.c)
#
#elseif(CMAKE_BUILD_TYPE STREQUAL "Debug")
#    target_include_directories(test1 PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/vendor/glad/debug/include/)
#    target_sources(test1 PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/vendor/glad/debug/src/glad.c)
#
#endif()

target_link_libraries(test1 zlib fmt::fmt)