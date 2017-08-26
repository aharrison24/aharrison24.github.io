---
layout: post
title: Using Boost in Compiler Explorer
tags: [c++, compiler-explorer, boost, godbolt]
---
You can use Boost in the [Godbolt Compiler Explorer](https://godbolt.org/) by
adding the correct include path in to the compiler flags:
```
-I/opt/compiler-explorer/libs/boost_1_64_0
```

It's not clear where the libs live, but presumably there's a way to link against those, too.

Here's an example showing the use of [Boost::hana](https://github.com/boostorg/hana)
within the compiler explorer:
[ https://godbolt.org/g/42CfS5](https://godbolt.org/g/42CfS5)
