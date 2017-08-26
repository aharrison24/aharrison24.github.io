---
layout: post
title: SFINAE for member detection
tags: [c++, c++98, c++03, sfinae, template]
---
Here's a C++98 compatible way to use SFINAE to check for a specific member variable on a type.

```c++
#include <boost/static_assert.hpp>

template <typename T>
struct has_member_a {
  typedef struct { char x[1]; } no;
  typedef struct { char x[2]; } yes;

  template <int> struct int_to_type;

  template <typename U> static yes check(int_to_type<sizeof(&U::a)>*);
  template <typename> static no check(...);
  
  static const bool value = (sizeof(check<T>(0)) == sizeof(yes));
};

struct S1 {
  int a;
  int b;
};

struct S2 {
  int A;
  int B;
};

int main() {
  BOOST_STATIC_ASSERT(has_member_a<S1>::value);
  BOOST_STATIC_ASSERT(!has_member_a<S2>::value);
}
```
Godbolt Compiler Explorer link: [https://godbolt.org/g/8ExaXg](https://godbolt.org/g/8ExaXg)

The `no`/`yes` types can be created more compactly by using something like:
```c++
  typedef char (&no) [1];
  typedef char (&yes) [2];
```
This feels more obfuscated though. You have to typedef a _reference_ to a fixed
size array, because the check function can't return a fixed size array by
value.

There's an excellent SFINAE tutorial on [Jean Guegant's Blog](http://jguegant.github.io/blogs/tech/sfinae-introduction.html).
Some simple macros to wrap the technique described here can be found at [Stack Overflow](https://stackoverflow.com/questions/36079170/how-to-check-if-a-member-name-variable-or-function-exists-in-a-class-with-or/36079999#36079999).


