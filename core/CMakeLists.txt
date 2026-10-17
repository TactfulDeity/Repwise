cmake_minimum_required(VERSION 3.25)

find_package(SQLiteCpp CONFIG REQUIRED)
find_package(nlohmann_json CONFIG REQUIRED)

add_library(core STATIC
    src/main.cpp
)

target_link_libraries(core PRIVATE
    nlohmann_json::nlohmann_json
    SQLiteCpp
)

target_include_directories(core PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

if(BUILD_CORE_TESTS)
    add_subdirectory(tests)
endif()