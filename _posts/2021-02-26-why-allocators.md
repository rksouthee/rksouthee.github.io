---
layout: post
title: "Why Allocators"
---

C++ allocators have a bad reputation and for good reason. Before C++11 they
couldn't have state and because they are used as a template parameter they
infect the type system. This means if you accept a generic container like
`std::vector`, but you only parameterize on the value type and not the
allocator type you may run into problems.

```cpp
// This may cause problems
template <typename T>
void f(const std::vector<T>&);

// Accept an allocator
template <typename T, typename A>
void g(const std::vector<T, A>&);
```

In an [interview](https://www.informit.com/articles/article.aspx?p=2314360)
with John Lakos, Alex Stepanov explains that allocators were added to the STL
"in order to get Microsoft to agree to consider including STL in the language".
The idea was to abstract a memory model allowing different pointer types, at
the time there were things like [far
pointers](https://en.wikipedia.org/wiki/Far_pointer).

Eventually allocators were seen as a way of customising how memory is
allocated. C++11 allowed allocators to have their own state and then C++17
introduced memory resources and polymorphic allocators.

In the same interview Alex says, "The whole point generic programming is to
make things simple, not to build everything-and-the-kitchen-sink policies". I
think this is an important point because the power of generic programming is
the ability to work with different types as long as they satisfy the same
requirements.
