---
layout: post
title: ! "Clang: Finding out which headers were included"
tags: [c++, clang, include, header]
---
From [Stack Overflow](https://stackoverflow.com/questions/5834778/how-to-tell-where-a-header-file-is-included-from).

Get gcc or clang to output a hierarchical list of included headers like this:
``` bash
clang -H ...
```
