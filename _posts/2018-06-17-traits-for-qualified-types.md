---
layout: post
title: Type traits and qualified types
tags: [c++, traits, cv-qualified]
---
# Type traits for cv-qualified and ref types

## Handling cv-qualified and ref types in type traits
When you write a type-trait based on specialization, it won't work correctly for
cv-qualified or reference types:

```c++
#include <type_traits>

template <int N>
struct foo {};

template <typename T>
struct is_foo : std::false_type {};

template <int N>
struct is_foo<foo<N>> : std::true_type {};

template <typename T>
constexpr bool is_foo_v = is_foo<T>::value;

// Works OK for plain type
static_assert(is_foo_v<foo<1>>);
static_assert(is_foo_v<foo<2>>);

// Doesn't work for const or ref types
static_assert(!is_foo_v<foo<1> const>);
static_assert(!is_foo_v<foo<2>&>);
```

## A tedious solution
One option is to provide further specializations for all the different
qualifiers:
```c++
template <int N>
struct is_foo<foo<N> const> : std::true_type {};

template <int N>
struct is_foo<foo<N>&> : std::true_type {};

template <int N>
struct is_foo<foo<N> const&> : std::true_type {};

template <int N>
struct is_foo<foo<N>&&> : std::true_type {};

template <int N>
struct is_foo<foo<N> const &&> : std::true_type {};

// ... we haven't even done 'volatile' yet
```
That gets old very quickly.

## A more elegant solution
As is often the case in programming, a more elegant answer lies in exploiting a
layer of indirection.

The following technique, borrowed from [Baptiste Wicht's ETL library](https://github.com/wichtounet/etl/blob/32e8153b38e0029176ca4fe2395b7fa6babe3189/include/etl/traits.hpp)
defines the basic type trait in a detail namespace, so that callers don't use it
directly. The library provides a variable template for use by the caller, but
crucially, the variable template applies the trait to `std::decay_t<T>`, to
ensure that cv and ref-qualified types are handled transparently:

```c++
#include <type_traits>

template <int N>
struct foo {};

namespace detail {

template <typename T>
struct is_foo_impl : std::false_type {};

template <int N>
struct is_foo_impl<foo<N>> : std::true_type {};

}  // namespace detail

template <typename T>
constexpr bool is_foo = detail::is_foo_impl<std::decay_t<T>>::value;

// Now handles types with extra qualifiers
static_assert(is_foo<foo<1>>);
static_assert(is_foo<foo<2>>);
static_assert(is_foo<foo<1> const>);
static_assert(is_foo<foo<2>&>);
static_assert(is_foo<foo<42> const volatile&&>);
```

## The standard library
The standard library uses a similar technique. This example is from the implementation of the placeholder types for `std::bind`:
```c++
template<class _Tp> struct __is_placeholder
    : public integral_constant<int, 0> {};
template<class _Tp> struct _LIBCPP_TYPE_VIS_ONLY is_placeholder
    : public __is_placeholder<typename remove_cv<_Tp>::type> {};

namespace placeholders
{

template <int _Np> struct __ph {};

_LIBCPP_FUNC_VIS extern __ph<1>   _1;
_LIBCPP_FUNC_VIS extern __ph<2>   _2;
_LIBCPP_FUNC_VIS extern __ph<3>   _3;
_LIBCPP_FUNC_VIS extern __ph<4>   _4;
_LIBCPP_FUNC_VIS extern __ph<5>   _5;
_LIBCPP_FUNC_VIS extern __ph<6>   _6;
_LIBCPP_FUNC_VIS extern __ph<7>   _7;
_LIBCPP_FUNC_VIS extern __ph<8>   _8;
_LIBCPP_FUNC_VIS extern __ph<9>   _9;
_LIBCPP_FUNC_VIS extern __ph<10> _10;

}  // placeholders

template<int _Np>
struct __is_placeholder<placeholders::__ph<_Np> >
    : public integral_constant<int, _Np> {};
```

The `__is_placeholder` struct is an implementation detail that callers aren't expected to use directly. The `is_placeholder` struct inherits from `__is_placeholder`, but removes cv-qualifiers from the type before passing it on.
