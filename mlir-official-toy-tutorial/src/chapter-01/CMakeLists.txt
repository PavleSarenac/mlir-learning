set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fno-rtti")

find_package(LLVM REQUIRED CONFIG
        PATHS /home/pavle-sarenac/Documents/mlir/mlir-beginner-friendly-tutorial/build/lib/cmake/llvm
)
find_package(MLIR REQUIRED CONFIG
        PATHS /home/pavle-sarenac/Documents/mlir/mlir-beginner-friendly-tutorial/build/lib/cmake/mlir
)

include_directories(${LLVM_INCLUDE_DIRS})
include_directories(${MLIR_INCLUDE_DIRS})
include_directories(include)

add_executable(toyc-chapter1
        toyc.cpp
        parser/AST.cpp
        include/toy/AST.h
        include/toy/Lexer.h
        include/toy/Parser.h
)

llvm_map_components_to_libnames(llvm_libs Support)

target_link_libraries(toyc-chapter1
        PRIVATE
        MLIRSupport
        ${llvm_libs}
)