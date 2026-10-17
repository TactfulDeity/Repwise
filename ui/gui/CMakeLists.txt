cmake_minimum_required(VERSION 3.25)

add_executable(repwise_gui
    main.cpp
)

target_link_libraries(repwise_gui
    core
)