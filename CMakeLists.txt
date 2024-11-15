cmake_minimum_required(VERSION 3.4)

project(SupSWFRecompiled)
set(CMAKE_C_STANDARD 17)
set(CMAKE_C_STANDARD_REQUIRED ON)

set(CMAKE_SKIP_BUILD_RPATH FALSE)
set(CMAKE_BUILD_WITH_INSTALL_RPATH TRUE)
set(CMAKE_INSTALL_RPATH "$\{ORIGIN\}")

set(RUNTIME_INCLUDES
    ${CMAKE_SOURCE_DIR}/lib/SWFModernRuntime/include
    ${CMAKE_SOURCE_DIR}/lib/SWFModernRuntime/include/actionmodern
    ${CMAKE_SOURCE_DIR}/lib/SWFModernRuntime/include/libswf
)

# RecompiledTags - Library containing the primary recompiler output
add_library(RecompiledTags STATIC)

#~ target_compile_options(RecompiledTags PRIVATE
    #~ # -Wno-unused-but-set-variable
    #~ -fno-strict-aliasing
    #~ -Wno-unused-variable
    #~ -Wno-implicit-function-declaration
#~ )

target_include_directories(RecompiledTags PRIVATE
    ${CMAKE_SOURCE_DIR}/include
    ${CMAKE_SOURCE_DIR}/RecompiledScripts
    ${RUNTIME_INCLUDES}
)

file(GLOB FUNC_C_SOURCES ${CMAKE_SOURCE_DIR}/RecompiledTags/*.c)

target_sources(RecompiledTags PRIVATE ${FUNC_C_SOURCES})

# RecompiledScripts - Library containing the secondary recompiler output for actions/abc
add_library(RecompiledScripts STATIC)

#~ target_compile_options(RecompiledScripts PRIVATE
    #~ # -Wno-unused-but-set-variable
    #~ -fno-strict-aliasing
    #~ -Wno-unused-variable
    #~ -Wno-implicit-function-declaration
#~ )

target_include_directories(RecompiledScripts PRIVATE
    ${CMAKE_SOURCE_DIR}/include
    ${RUNTIME_INCLUDES}
)

file(GLOB FUNC_C_SOURCES ${CMAKE_SOURCE_DIR}/RecompiledScripts/*.c)

target_sources(RecompiledScripts PRIVATE ${FUNC_C_SOURCES})

add_executable(${PROJECT_NAME})

set(SOURCES
    ${CMAKE_SOURCE_DIR}/src/main.c
)

target_compile_options(${PROJECT_NAME} PRIVATE -O2)

set(RENAME_ZCONF OFF)

add_subdirectory(${CMAKE_SOURCE_DIR}/lib/SWFModernRuntime)

target_link_libraries(${PROJECT_NAME} PRIVATE
    RecompiledTags
    RecompiledScripts
    SWFModernRuntime
)

target_sources(${PROJECT_NAME} PRIVATE ${SOURCES})
target_include_directories(${PROJECT_NAME} PRIVATE
    ${CMAKE_SOURCE_DIR}/include
    ${RUNTIME_INCLUDES}
)