---
layout: post
title: Finding out the dependencies of a CMake project
tags: [CMake, dependencies, c++]
---
I’m not at all proud of this. In fact I think it is horrific. But it will do a reasonable job of pulling out the first argument of all the `find_package` calls in your CMake code.

``` bash
find . -iname "CMakeLists.txt" -o -iname "*.cmake*" | xargs grep -Eih "^\s*find_package\s*\(" | tr '[:upper:]' '[:lower:]' | sed -e "s/.*find_package.*([^a-z0-9]*\([a-z0-9]*\).*).*/\1/" | sort | uniq
```
