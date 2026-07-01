set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fno-rtti")

find_package(LLVM REQUIRED CONFIG
        PATHS /home/pavle-sarenac/Documents/mlir/mlir-beginner-friendly-tutorial/build/lib/cmake/llvm
)
find_package(MLIR REQUIRED CONFIG
        PATHS /home/pavle-sarenac/Documents/mlir/mlir-beginner-friendly-tutorial/build/lib/cmake/mlir
)

list(APPEND CMAKE_MODULE_PATH "${MLIR_CMAKE_DIR}" "${LLVM_CMAKE_DIR}")
include(TableGen)
include(AddLLVM)
include(AddMLIR)

set(LLVM_LINK_COMPONENTS
        Support
)

include_directories(${LLVM_INCLUDE_DIRS})
include_directories(${MLIR_INCLUDE_DIRS})
include_directories(include)
include_directories(${CMAKE_CURRENT_BINARY_DIR})
include_directories(${CMAKE_CURRENT_BINARY_DIR}/include)

set(LLVM_TARGET_DEFINITIONS mlir/ToyCombine.td)
mlir_tablegen(ToyCombine.inc -gen-rewriters)
add_public_tablegen_target(ToyCh3CombineIncGen)

add_subdirectory(include)

add_executable(toyc-chapter3
        toyc.cpp
        include/toy/AST.h
        include/toy/Dialect.h
        include/toy/Lexer.h
        include/toy/MLIRGen.h
        include/toy/Parser.h
        mlir/Dialect.cpp
        mlir/MLIRGen.cpp
        mlir/ToyCombine.cpp
        parser/AST.cpp
)

llvm_map_components_to_libnames(llvm_libs Support)

target_link_libraries(toyc-chapter3
        PRIVATE
        MLIRAnalysis
        MLIRCallInterfaces
        MLIRCastInterfaces
        MLIRFunctionInterfaces
        MLIRIR
        MLIRParser
        MLIRPass
        MLIRSideEffectInterfaces
        MLIRSupport
        MLIRTransforms
        ${llvm_libs}
)

add_dependencies(toyc-chapter3 ToyCh3OpsIncGen ToyCh3CombineIncGen)
