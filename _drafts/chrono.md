---
layout: post
title: "Using `<chrono>`"
---

# TODO

* maybe turn this into a tutorial/guide?
* lossy conversion
* verbosity
* printing (support is being added)
* efficiency (assembly output)
* using `std::chrono::steady_clock` rather than `std::chrono::high_resolution_clock`

I first started using the `<chrono>` library as a way for timing code. The way I used the library was based
on examples I found online in various blogs and videos. After a while I was curious about the design decisions
that led to `<chrono>` and so I watched some videos by Howard Hinnant. The videos were very informative and helped
me learn about the design of `<chrono>` and how to use it properly.

An example based on the "Programming a Wireless Robotic Arm" video by javix9. The goal is to print a period of 20ms.

```cpp
#include <iostream>

void period(float seconds)
{
	std::cout << seconds << "s\n";
}

int main()
{
	period(0.02f);
}
```

to

```cpp
#include <iostream>
#include <chrono>

using namespace std::literals;

void period(std::chrono::duration<float> seconds)
{
	std::cout << seconds.count() << "s\n";
}

int main()
{
	period(20ms);
}
```
