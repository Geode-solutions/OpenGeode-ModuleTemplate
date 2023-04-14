# Copyright (c) 2019 - 2022 Geode-solutions
#
# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in
# all copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
# SOFTWARE.

cmake_minimum_required(VERSION 3.15)

cmake_policy(SET CMP0091 NEW)

# Define the project
project(OpenGeode-MyModule CXX)

option(OPENGEODE_MYMODULE_WITH_TESTS "Compile test projects" ON)
option(OPENGEODE_MYMODULE_WITH_PYTHON "Compile Python bindings" OFF)

# Get MyModule dependencies
find_package(OpenGeode REQUIRED CONFIG)

#------------------------------------------------------------------------------------------------
# Configure the MyModule libraries
add_geode_library(
    NAME mylib
    FOLDER "mylib"
    SOURCES
        "common.cpp"
        "hello_world.cpp"
    PUBLIC_HEADERS
        "hello_world.h"
    PRIVATE_DEPENDENCIES
        OpenGeode::basic
)

#------------------------------------------------------------------------------------------------
# Optional modules configuration
if(OPENGEODE_MYMODULE_WITH_TESTS)
    # Enable testing with CTest
    enable_testing()
    message(STATUS "Configuring OpenGeode-MyModule with tests")
    add_subdirectory(tests)
endif()

if(OPENGEODE_MYMODULE_WITH_PYTHON)
    message(STATUS "Configuring OpenGeode-MyModule with Python bindings")
    add_subdirectory(bindings/python)
endif()

#------------------------------------------------------------------------------------------------
# Configure CPacks
if(WIN32)
    set(CPACK_GENERATOR "ZIP")
else()
    set(CPACK_GENERATOR "TGZ")
endif()

# This must always be last!
include(CPack)
