set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fno-rtti")

find_package(LLVM REQUIRED CONFIG
        PATHS /home/pavle-sarenac/Documents/mlir/mlir-beginner-friendly-tutorial/build/lib/cmake/llvm
)
find_package(MLIR REQUIRED CONFIG
        PATHS /home/pavle-sarenac/Documents/mlir/mlir-beginner-friendly-tutorial/build/lib/cmake/mlir
)

include_directories(${LLVM_INCLUDE_DIRS})
include_directories(${MLIR_INCLUDE_DIRS})
include_directories(toy/include)

add_executable(toyc-chapter1
        toyc.cpp
        toy/parser/AST.cpp
        toy/include/toy/AST.h
        toy/include/toy/Lexer.h
        toy/include/toy/Parser.h
)

llvm_map_components_to_libnames(llvm_libs Support Core)

message(STATUS "LLVM libs: ${llvm_libs}")

target_link_libraries(toyc-chapter1
        PRIVATE
        MLIRSupport
        ${llvm_libs}
)