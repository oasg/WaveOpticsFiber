cmake_minimum_required(VERSION 3.16)
set (CMAKE_CXX_STANDARD 14)
project("Sim")

# set(CMAKE_BUILD_TYPE Release)
set(CMAKE_CXX_FLAGS_DEBUG "-g")
set(CMAKE_CXX_FLAGS_RELEASE "-O3")

find_package(OpenMP)
if (OPENMP_FOUND)
    set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
    set (CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
endif()
include_directories(eigen3)
add_library(solver MoM_ob.cpp Solver.cpp)
target_link_libraries(solver)
add_executable(Sim genDist.cpp)

#link the library libcomplex_bessel
add_subdirectory(complex_bessel)
include_directories(complex_bessel/include)
target_link_libraries(Sim solver complex_bessel::complex_bessel)
