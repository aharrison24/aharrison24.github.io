---
layout: post
title: Adding a cmakelint target to a CMake project
tags: [cmake, cmakelint, c++]
---
First install the [cmakelint](https://github.com/richq/cmake-lint) tool like this:

``` bash
pip install cmakelint
```

Then add a linting target to your CMake project:

``` cmake
find_program(CMAKELINT_EXECUTABLE NAMES cmakelint cmakelint.py)
if(CMAKELINT_EXECUTABLE)
    file(GLOB_RECURSE ALL_CMAKE_FILES CMakeLists.txt *.cmake)
    add_custom_target(
        cmakelint
        COMMAND ${CMAKELINT_EXECUTABLE} ${ALL_CMAKE_FILES}
        COMMENT "Running cmakelint tool"
        VERBATIM
    )
endif()
```
