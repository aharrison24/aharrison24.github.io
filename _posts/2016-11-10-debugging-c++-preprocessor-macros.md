---
layout: post
title: Debugging C++ preprocessor macros
tags: [c++, macros, gcc, clang, coliru]
---
It's possible to get clang / g++ to run only the preprocessor and to strip away all the cruft that comes in with headers.

```bash
$ g++ -std=c++11 -E -P -c main.cpp
```

From the documentation of g++:

```
 -E     Run the preprocessor stage.
 -P     Tell the preprocessor not to generate `#line' directives.
        Used with the `-E' option.
 -c     Don't run the linker
```

An excellent way to get online feedback on your changes is to use a service like Coliru. See an example of preprocessor output at
http://coliru.stacked-crooked.com/a/ce86de4f956f9316
