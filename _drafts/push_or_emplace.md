---
layout: post
title: "push_back or emplace_back"
---

In this post I want to quickly go over how I use `push_back` or `emplace_back`
when using something like `std::vector`. I use `push_back` when I have an
existing object and I want to copy or move it into a container. I use
`emplace_back` when I don't have an object and I want to construct a new one
directly in the container, this can help performance since a temporary object
doesn't need to be created outside the container.

Although it's possible to use `emplace_back` to pass existing objects as
arguments, the problem is that `emplace_back` is a variadic template function
and can slow down compilation.  Alternatively `push_back` has two overloads
that take `const value_type&` or `value_type&&`.

The following is some code demonstrating how I typically use `push_back` and
`emplace_back`.

```cpp
#include <cassert>
#include <iostream>
#include <string>
#include <vector>

int main()
{
	std::vector<std::string> strings;
	std::string str = "Hello";

	strings.push_back(str); // make a copy
	assert(str == "Hello");

	str += ", World!";
	// don't need str anymore, move it into strings
	strings.push_back(std::move(str));

	strings.emplace_back(3, 'X'); // construct a string directly

	for (const auto& s : strings)
		std::cout << s << '\n';
}
```
