cmake_minimum_required(VERSION 3.4)

project(SupSWFRecompiled)
set(CMAKE_C_STANDARD 17)
set(CMAKE_C_STANDARD_REQUIRED ON)

set(CMAKE_SKIP_BUILD_RPATH FALSE)
set(CMAKE_BUILD_WITH_INSTALL_RPATH TRUE)
set(CMAKE_INSTALL_RPATH "$\{ORIGIN\}")

add_executable(${PROJECT_NAME})

set(SOURCES
    ${CMAKE_SOURCE_DIR}/src/main.c
)

target_compile_options(${PROJECT_NAME} PRIVATE -O2)

set(RENAME_ZCONF OFF)

add_subdirectory(${CMAKE_SOURCE_DIR}/lib/SWFModernRuntime)

target_link_libraries(${PROJECT_NAME} PRIVATE
    SWFModernRuntime
)

target_sources(${PROJECT_NAME} PRIVATE ${SOURCES})
target_include_directories(${PROJECT_NAME} PRIVATE
    ${CMAKE_SOURCE_DIR}/include
    lib/SWFModernRuntime/include/libswf
)