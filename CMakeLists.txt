cmake_minimum_required(VERSION 3.5)
project(lesson_individual)

add_executable(canapp canapp.cpp ccondvar.cpp)
add_executable(leading leading.cpp ccondvar.cpp)

target_include_directories(
    canapp PUBLIC
    "${CMAKE_CURRENT_SOURCE_DIR}/include"
    "${CMAKE_CURRENT_SOURCE_DIR}"
    "${CMAKE_CURRENT_SOURCE_DIR}/canopenmaster/include"
)

target_include_directories(
    leading PUBLIC
    "${CMAKE_CURRENT_SOURCE_DIR}/include"
)

set_target_properties(canapp PROPERTIES CXX_STANDARD 17 CXX_STANDARD_REQUIRED ON)

add_library(ccanopenmaster SHARED IMPORTED)

#TODO made path relative
execute_process (
    COMMAND bash -c "cp /nvme0n1p4/home/chuv/git/LEARNING/OTUS_CPP/lesson_individual/canopenmaster/build/libccanopenmaster.so /nvme0n1p4/home/chuv/git/LEARNING/OTUS_CPP/lesson_individual/build/"
    OUTPUT_VARIABLE outVar
)

set_property(TARGET ccanopenmaster PROPERTY
             IMPORTED_LOCATION "libccanopenmaster.so")

SET(CMAKE_EXE_LINKER_FLAGS 
          "${CMAKE_EXE_LINKER_FLAGS} -Wl,-rpath -Wl,.")

target_link_libraries(	canapp PRIVATE  ccanopenmaster)

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pthread")

target_compile_options(
    canapp PRIVATE
    -Wall -Wextra -pedantic -Werror -Wno-unused-local-typedefs -g
)