cmake_minimum_required(VERSION 4.1)
project(mlir_official_toy_tutorial)

set(CMAKE_CXX_STANDARD 17)

add_subdirectory(src/chapter-01)
add_subdirectory(src/chapter-02)

add_executable(mlir_official_toy_tutorial main.cpp)
