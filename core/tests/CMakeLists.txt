cmake_minimum_required(VERSION 3.25)

find_package(Catch2 CONFIG REQUIRED)

add_executable(repwise_tests
    main.cpp
)

target_link_libraries(repwise_tests PRIVATE 
    Catch2::Catch2WithMain
)

include(Catch)
catch_discover_tests(repwise_tests)