cmake_minimum_required(VERSION 3.25)

add_executable(repwise_tui
    main.cpp
)

target_link_libraries(repwise_tui
    core
)