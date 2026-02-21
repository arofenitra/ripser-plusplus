cmake_minimum_required(VERSION 3.18)
project(pyripser++ LANGUAGES CXX CUDA)

# Set build type
set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Build type")

# Enable C++11 and CUDA standards
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CUDA_STANDARD 11)

# Include directories
include_directories(ripserplusplus/include)

# OpenMP support
find_package(OpenMP REQUIRED)
if(OpenMP_FOUND)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
endif()

# Shared library for phmap
add_library(phmap SHARED ./ripserplusplus/include/phmap_interface/phmap_interface.cpp)

# Main CUDA library
add_library(pyripser++ SHARED ./ripserplusplus/ripser++.cu)

# Link phmap to pyripser++
target_link_libraries(pyripser++ PRIVATE phmap)

# Set CUDA architectures (Titan RTX = 7.5)
set_property(TARGET pyripser++ PROPERTY CUDA_ARCHITECTURES 75)
