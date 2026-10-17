cmake_minimum_required(VERSION 3.25)

project (Repwise
VERSION 0.1
LANGUAGES CXX
)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(BUILD_CORE_TESTS "Build core tests" ON)
option(BUILD_CLI "Build CLI" OFF)
option(BUILD_TUI "Build TUI" OFF)
option(BUILD_GUI "Build GUI" OFF)

# Make sure CMake options and vcpkg features agree.
if(BUILD_CORE_TESTS AND NOT "tests" IN_LIST VCPKG_MANIFEST_FEATURES)
    message(FATAL_ERROR
        "BUILD_CORE_TESTS=ON requires VCPKG_MANIFEST_FEATURES to include 'tests'.")
endif()

if(BUILD_CLI AND NOT "cli" IN_LIST VCPKG_MANIFEST_FEATURES)
    message(FATAL_ERROR
        "BUILD_CLI=ON requires VCPKG_MANIFEST_FEATURES to include 'cli'.")
endif()

if(BUILD_TUI AND NOT "tui" IN_LIST VCPKG_MANIFEST_FEATURES)
    message(FATAL_ERROR
        "BUILD_TUI=ON requires VCPKG_MANIFEST_FEATURES to include 'tui'.")
endif()

if(BUILD_GUI AND NOT "gui" IN_LIST VCPKG_MANIFEST_FEATURES)
    message(FATAL_ERROR
        "BUILD_GUI=ON requires VCPKG_MANIFEST_FEATURES to include 'gui'.")
endif()

add_subdirectory(core)

if(BUILD_CLI)
    add_subdirectory(ui/cli)
endif()

if(BUILD_TUI)
    add_subdirectory(ui/tui)
endif()

if(BUILD_GUI)
    add_subdirectory(ui/gui)
endif()
