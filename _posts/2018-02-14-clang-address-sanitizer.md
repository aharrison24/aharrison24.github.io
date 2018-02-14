---
layout: post
title: Useful flags for clang address sanitizer
tags: [c++, clang, address, sanitizer, asan]
---
From [Loius Brandy's CppCon 2017 talk](https://www.youtube.com/watch?v=lkgszkPnV8g).

Asan has a 'use after scope' option which is not enabled by default (until recently). Turn it on with `-fsanitize-address-use-after-scope`.

Catches bugs like this one:
``` c++
volatile int *p = 0;

int main() {
  {
    int x = 0;
    p = &x;
  }
  *p = 5;
  return 0;
}
```
