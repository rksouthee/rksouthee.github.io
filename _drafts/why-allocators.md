---
layout: post
title:  "Why Allocators"
---

C++ allocators have a bad reputation, and for good reason, they couldn't have state before C++11, and because
they are used as a template parameter to the standard containers, they infect the type system. In an
[interview](https://www.informit.com/articles/article.aspx?p=2314360) with John Lakos, Alex Stepanov explains
that allocators were added to the STL "in order to get Microsoft to agree to consider including STL in the
language". The idea was to abstract the memory model, what is a pointer/reference, so that the STL would support
things like [far pointers](https://en.wikipedia.org/wiki/Far_pointer).

At some point, allocators were seen as a way of customizing the memory allocation of the standard containers.
This has now led to memory resources and `std::polymorphic_allocator`. If I were to design the STL today, I
wouldn't support the allocator template model, and I would not support the polymorphic allocator model with
the standard containers. As Alex said in the interview "The whole point generic programming is to make things
simple, not to build everything-and-the-kitchen-sink policies".
