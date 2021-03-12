---
layout: post
title: "Perfect Forwarding"
---

```cpp
#include <iostream>
#include <utility>

/* Perfect forwarding */

void print_value(const int&)
{
	std::cout << "lvalue\n";
}

void print_value(int&&)
{
	std::cout << "rvalue\n";
}

template <typename T>
void print(T&& x)
{
	print_value(std::forward<T>(x));
}

int main()
{
	int value = 3;
	print(value); // lvalue
	print(std::move(value)); // rvalue
}
```
