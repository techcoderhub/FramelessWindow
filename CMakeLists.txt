cmake_minimum_required(VERSION 3.20)

project(FramelessHelper VERSION 0.1 LANGUAGES CXX)

set(FLUENTUI_DIRECTORY ${CMAKE_CURRENT_LIST_DIR})
list(APPEND CMAKE_MODULE_PATH ${FLUENTUI_DIRECTORY}/.cmake/)

include(GetGitRevisionDescription)

option(FLUENTUI_BUILD_EXAMPLES "Build FluentUI demo applications." ON)
option(FLUENTUI_BUILD_STATIC_LIB "Build static library." OFF)

find_package(QT NAMES Qt6 Qt5 REQUIRED COMPONENTS Core)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Core)

set(QT_SDK_DIR "${Qt${QT_VERSION_MAJOR}_DIR}")
cmake_path(GET QT_SDK_DIR PARENT_PATH QT_SDK_DIR)
cmake_path(GET QT_SDK_DIR PARENT_PATH QT_SDK_DIR)
cmake_path(GET QT_SDK_DIR PARENT_PATH QT_SDK_DIR)

if (NOT CMAKE_CXX_STANDARD)
    set (CMAKE_CXX_STANDARD 17)
endif()

set(FRAMELESSHELPER_BUILD_STATIC ON)
set(FRAMELESSHELPER_NO_DEBUG_OUTPUT ON)
set(FRAMELESSHELPER_BUILD_WIDGETS OFF)
add_subdirectory(framelesshelper)
add_subdirectory(start)

message("------------------------ FluentUI ------------------------")
message("Build Frameless Window demo applications.: ${FLUENTUI_BUILD_EXAMPLES}")
message("Build static library.: ${FLUENTUI_BUILD_STATIC_LIB}")

