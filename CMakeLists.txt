cmake_minimum_required(VERSION 2.8.12)
project(bop_renderer)


################################################################################
# Before running cmake, set the following environment variables:

# OSMESA_PREFIX - Folder with the OSMesa installation
# PYTHON_PREFIX - Folder with "lib" and "include" subfolders with Python sources

# Example:
# export OSMESA_PREFIX=/opt/osmesa
# export PYTHON_PREFIX=~/.conda/envs/env_name
################################################################################

set(LLVM_DIR "/opt/llvm")
set(OSMESA_DIR "/opt/osmesa")
set(OSMESA_PREFIX "/opt/osmesa")
#set(OSMESA_INCLUDE_DIRS "/opt/osmesa/include/GL")
set(PYTHON_PREFIX "/usr/lib/x86_64-linux-gnu")
#set(PYTHON_INCLUDE_DIRS "/usr/include/python3.8")

# Enable support for C++11.
set(CPP_STANDARD -std=c++11)
add_definitions(${CPP_STANDARD})

# Python library (tested with Python 3.6).
set(PYTHON_LIBRARIES $ENV{PYTHON_PREFIX}/lib/libpython3.8.so)

# Directory with Python includes (tested with Python 3.6).
#set(PYTHON_INCLUDE_DIRS $ENV{PYTHON_PREFIX}/include/python3.8)

# OSMesa.
set(OSMESA_LIBRARIES $ENV{OSMESA_PREFIX}/lib/libOSMesa32.so)
include_directories($ENV{OSMESA_PREFIX}/include)
#include_directories($ENV{OSMESA_PREFIX}/include/GL)
#link_directories(/opt/osmesa/lib)

#target_include_directories(test PRIVATE ${OSMESA_INCLUDE_DIRS})
#set(SOURCES /app/bop_renderer/src/context/OSMContext.cpp ${OSMESA_INCLUDE_DIRS}/osmesa.h)
#add_executable(test ${SOURCES})


# Pybind11.
set(PYBIND11_CPP_STANDARD ${CPP_STANDARD})
set(PYBIND11_PYTHON_VERSION 3.8)
add_subdirectory(3rd/pybind11)

# Others.
include_directories(src)
include_directories(3rd)
include_directories(3rd/glm)

# Header files.
set(HEADERFILES
        src/Renderer.h
        src/PythonWrapper.h
        src/AbstractLoader.h
        src/PlyLoader.h
        src/GPUBuffer.h
        src/Pose.h
        src/Geometry.h
        src/glutils/FrameBufferObject.h
        src/glutils/GLSLProgram.h
        src/glutils/gl_core_3_3.h
        src/context/Context.h
        src/Texture.h
        src/Shaders.h
        #/opt/osmesa/include/GL/osmesa.h
        )

# Source files.
set(SOURCEFILES
        src/Renderer.cpp
        src/AbstractLoader.cpp
        src/PlyLoader.cpp
        src/GPUBuffer.cpp
        src/Pose.cpp
        src/glutils/FrameBufferObject.cpp
        src/glutils/GLSLProgram.cpp
        src/glutils/gl_core_3_3.cpp
        src/context/OSMContext.cpp
        3rd/rply/rply.c
        3rd/lodepng/lodepng.cpp
        )

# Python module.
pybind11_add_module(bop_renderer MODULE
        src/PythonWrapper.cpp ${HEADERFILES} ${SOURCEFILES})
target_link_libraries(bop_renderer PRIVATE ${OSMESA_LIBRARIES})
