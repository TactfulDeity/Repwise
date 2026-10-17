cmake_minimum_required(VERSION 3.25)

find_package(cli11 CONFIG REQUIRED)

add_executable(repwise_cli
    main.cpp
)

target_link_libraries(repwise_cli
    core
    cli11::cli11
)