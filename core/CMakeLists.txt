cmake_minimum_required(VERSION 3.25)

find_package(SQLiteCpp CONFIG REQUIRED)

add_library(core STATIC
    src/main.cpp
)

target_link_libraries(core PRIVATE
    SQLiteCpp::SQLiteCpp
)

target_include_directories(core PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

if(BUILD_CORE_TESTS)
    add_subdirectory(tests)
endif()