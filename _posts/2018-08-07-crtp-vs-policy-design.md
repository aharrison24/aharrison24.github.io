---
layout: post
title: ! 'CRTP vs Policy design tradeoffs'
tags: [c++,crtp,policy,policies]
---
I've often pondered the advantages and disadvantages of CRTP vs a policy based design. CRTP always seemed
a bit of a hack to me, so I've favoured policies. I recently came across an interesting rationale for
choosing CRTP in the Boost `iterator_facade` documentation at
[https://www.boost.org/doc/libs/1_65_0/libs/iterator/doc/iterator_facade.html#overview](https://www.boost.org/doc/libs/1_65_0/libs/iterator/doc/iterator_facade.html#overview)

Here's the extract:
> Iterator facade uses the Curiously Recurring Template Pattern (CRTP) [Cop95] so that the user can specify the
> behavior of `iterator_facade` in a derived class. Former designs used policy objects to specify the behavior,
> but that approach was discarded for several reasons:
>
> 1. the creation and eventual copying of the policy object may create overhead that can be
>    avoided with the current approach.
> 2. The policy object approach does not allow for custom constructors on the created iterator
>    types, an essential feature if `iterator_facade` should be used in other library implementations.
> 3. Without the use of CRTP, the standard requirement that an iterator's operator++ returns the
>    iterator type itself would mean that all iterators built with the library would have to be
>    specializations of `iterator_facade<...>`, rather than something more descriptive like
>    `indirect_iterator<T*>`. Cumbersome type generator metafunctions would be needed to build
>    new parameterized iterators, and a separate `iterator_adaptor` layer would be impossible.
>
> [Cop95] [Coplien, 1995] Coplien, J., Curiously Recurring Template Patterns, C++ Report, February 1995, pp. 24-27.

