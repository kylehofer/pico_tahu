cmake_minimum_required(VERSION 3.17)

SET(BUILD_TARGET PICO CACHE BOOL "")
SET(PICO_TAHU_STATIC ON CACHE BOOL "")
SET(PICO_TAHU_SHARED OFF CACHE BOOL "")

IF(${BUILD_TARGET} STREQUAL "PICO")
    include(pico_sdk_import.cmake)
    set(PICO_BOARD pico_w)
ELSE()
ENDIF()

project(pico_tahu C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 20)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -fPIC")
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -g")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -O3")

IF(${BUILD_TARGET} STREQUAL "PICO")
    pico_sdk_init()
ENDIF()

# Finding all of our source files
file(GLOB_RECURSE SOURCES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} "src/*.c")

# Main library compiling
IF(${PICO_TAHU_STATIC})
    add_library(pico_tahu STATIC ${SOURCES})
ELSE()
    add_library(pico_tahu SHARED ${SOURCES})
ENDIF()

IF(${BUILD_TARGET} STREQUAL "PICO")
    target_link_libraries(pico_tahu
        pico_stdlib)
ENDIF()

target_include_directories(pico_tahu PUBLIC "src/")

IF(${BUILD_TARGET} STREQUAL "PICO")
    add_definitions(-DPICO)
ENDIF()