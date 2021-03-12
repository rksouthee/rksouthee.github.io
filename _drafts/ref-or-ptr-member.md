---
layout: post
title: "Reference or Pointer as data member"
---

```cpp
#include <iostream>

struct Ref_mem {
	int& ref;
};

struct Ptr_mem {
	int* ptr;

	Ptr_mem() = default;

	explicit Ptr_mem(int& x) :
		ptr{&x}
	{
	}
};

int main()
{
	/* Ref_mem ra; // error: use of deleted function Ref_mem::Ref_mem() */
	Ptr_mem pa;

	int value = 42;
	Ref_mem rb{value};
	Ptr_mem pb{value};
	std::cout << rb.ref << '\n';
	std::cout << *pb.ptr << '\n';

	++value;
	Ref_mem rc = rb;
	Ptr_mem pc = pb;
	std::cout << rc.ref << '\n';
	std::cout << *pc.ptr << '\n';
}
```
