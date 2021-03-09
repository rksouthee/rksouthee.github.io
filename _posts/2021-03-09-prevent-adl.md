---
layout: post
title: "Prevent ADL with static member functions"
---

This is a quick tip that shows how you can use static member functions as an
alternative to free functions if you would like to prevent ADL
(Argument-dependent lookup). Although ADL is useful, particularly for finding
necessary operators, it can be a little too helpful, finding functions that you
didn't intend.  Using static member functions can help the compiler catch
unqualified names being looked up, and you have a better idea of which function
is being called.

```cpp
#include <iostream>

namespace my_lib {

class My_class { };

class Util {
public:
	static void print_static(const My_class&)
	{
		std::cout << "print_static" << std::endl;
	}
};

void print_adl(const My_class&)
{
	std::cout << "print_adl" << std::endl;
}

} // namespace my_lib

int main()
{
	my_lib::My_class x;
	print_adl(x); // OK Argument-dependent lookup
	/* print_static(x); // Error: print_static not declared in this scope */
	my_lib::Util::print_static(x);
}
```
