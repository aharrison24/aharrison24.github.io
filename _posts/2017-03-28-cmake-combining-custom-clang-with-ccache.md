---
layout: post
title: ! 'CMake: Combining use of a custom Clang build with ccache'
tags: [cmake, ccache, clang, c++]
---
If you’ve built a custom version of LLVM and clang, you can make a file containing paths to the compiler binaries that you want to use. This is like a poor-man’s toolchain file.

Create a file that looks like this. I called it `llvm_tool_paths.cmake`:
```cmake
# Include in build with:
# cmake -C"llvm_tool_paths.cmake" ...

# Where to find the ccache binary
set(CCACHE_BIN "/usr/local/bin/ccache")

# Where to find the LLVM binaries
# NOTE: YOU NEED TO SET THIS
set(LLVM_BIN_DIR "/llvm/install/bin")

# Tell CMake to use ccache to launch the compiler
# Works on CMake 3.4 and above
set(CMAKE_CXX_COMPILER_LAUNCHER "${CCACHE_BIN}" CACHE STRING "")

# Compiler and linker paths
set(CMAKE_C_COMPILER "${LLVM_BIN_DIR}/clang" CACHE STRING "")
set(CMAKE_CXX_COMPILER "${LLVM_BIN_DIR}/clang++" CACHE STRING "")
set(CMAKE_LINKER "${LLVM_BIN_DIR}/clang++" CACHE STRING "")

# Other things you might consider setting
#set(CMAKE_AR "${LLVM_BIN_DIR}/llvm-ar" CACHE STRING "")
#set(CMAKE_NM "${LLVM_BIN_DIR}/llvm-nm" CACHE STRING "")
#set(CMAKE_OBJDUMP "${LLVM_BIN_DIR}/llvm-objdump" CACHE STRING "")
#set(CMAKE_RANLIB "${LLVM_BIN_DIR}/llvm-ranlib" CACHE STRING "")
#set(_CMAKE_TOOLCHAIN_PREFIX "llvm-" CACHE STRING "")
```

Now invoke cmake like this:
```bash
cmake ../src -C"/path/to/llvm_tool_paths.cmake"
```

### Troubleshooting
#### `CMAKE_CXX_STANDARD`
If you want to combine this technique with use of CMAKE_CXX_STANDARD then command order is very important.

``` cmake
# Must come before project command, as it sets policy CMP0025,
# which is necessary to detect clang version correctly.
cmake_minimum_required(VERSION 3.5)
# Enable C++14 features on gcc/clang
# These must come *before* the 'project' command, as the 'project'
# command performs compiler checks.
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(MyProject)
```

### Further reading
https://public.kitware.com/Bug/view.php?id=15943
https://crascit.com/2016/04/09/using-ccache-with-cmake/
https://cmake.org/Wiki/CMake_Cross_Compiling#The_toolchain_file
http://stackoverflow.com/questions/1815688/how-to-use-ccache-with-cmake
