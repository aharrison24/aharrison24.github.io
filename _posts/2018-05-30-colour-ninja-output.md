---
layout: post
title: Colour output from Ninja generator in CMake
tags: [cmake, ninja, c++]
---

Found in the [BloatyMcBloatFace](https://github.com/google/bloaty/blob/2adf8706bdfa4ae93d43e4f829f53fa7c0983e0e/CMakeLists.txt#L52)
repository is this snippet to get coloured compiler output when using the Ninja
generator from CMake.

```cmake
# When using Ninja, compiler output won't be colorized without this.
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG(-fdiagnostics-color=always SUPPORTS_COLOR_ALWAYS)
if(SUPPORTS_COLOR_ALWAYS)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fdiagnostics-color=always")
endif()
```
