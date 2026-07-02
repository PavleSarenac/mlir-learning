# This chapter depends on JIT support enabled.
if (NOT MLIR_ENABLE_EXECUTION_ENGINE)
    return()
endif ()

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
        Core
        Support
        nativecodegen
        OrcJIT
)

include_directories(${LLVM_INCLUDE_DIRS})
include_directories(${MLIR_INCLUDE_DIRS})
include_directories(include)
include_directories(${CMAKE_CURRENT_BINARY_DIR})
include_directories(${CMAKE_CURRENT_BINARY_DIR}/include)
add_subdirectory(include)

set(LLVM_TARGET_DEFINITIONS mlir/ToyCombine.td)
mlir_tablegen(ToyCombine.inc -gen-rewriters)
add_public_tablegen_target(ToyCh7CombineIncGen)

add_executable(toyc-chapter7
        toyc.cpp
        include/toy/AST.h
        include/toy/Dialect.h
        include/toy/Lexer.h
        include/toy/MLIRGen.h
        include/toy/Parser.h
        mlir/Dialect.cpp
        mlir/LowerToAffineLoops.cpp
        mlir/ShapeInferencePass.cpp
        mlir/ToyCombine.cpp
        mlir/MLIRGen.cpp
        mlir/LowerToLLVM.cpp
        parser/AST.cpp
)


get_property(dialect_libs GLOBAL PROPERTY MLIR_DIALECT_LIBS)
get_property(conversion_libs GLOBAL PROPERTY MLIR_CONVERSION_LIBS)
get_property(extension_libs GLOBAL PROPERTY MLIR_EXTENSION_LIBS)

target_link_libraries(toyc-chapter7
        PRIVATE
        ${dialect_libs}
        ${conversion_libs}
        ${extension_libs}
        MLIRAnalysis
        MLIRBuiltinToLLVMIRTranslation
        MLIRCallInterfaces
        MLIRCastInterfaces
        MLIRExecutionEngine
        MLIRFunctionInterfaces
        MLIRIR
        MLIRLLVMCommonConversion
        MLIRLLVMDialect
        MLIRLLVMToLLVMIRTranslation
        MLIRMemRefDialect
        MLIRParser
        MLIRPass
        MLIRSideEffectInterfaces
        MLIRSupport
        MLIRTargetLLVMIRExport
        MLIRTransforms
        ${llvm_libs}
)

add_dependencies(toyc-chapter6 ToyCh7ShapeInferenceInterfaceIncGen ToyCh7OpsIncGen ToyCh7CombineIncGen)