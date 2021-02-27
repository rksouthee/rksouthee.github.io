---
layout: post
title: "Using the chrono library"
---

I first started using the chrono library as a way to time code and my usage was
based on blogs and tutorials.  After a while I was curious about the design of
the chrono library, which led me to watch some videos by the designer Howard
Hinnant.  The videos were very informative and helped me learn about the design
of chrono and how to use it properly.

Although the chrono library introduced clocks and time points, I think what
helped me most was understanding how to use durations. Once understood I had a
better understanding of the other components of the library.

In the following example, the goal is to pass a value of 20ms to `period` which
will then print the result.

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

We have to keep track of what units the function expects the argument and
performing the conversion ourselves.  Using the chrono library allows to
express our intent using the type system and user-defined literals.

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

In this example we express our content using the type system. We also take
advantage of user-defined literals to call `period` with 20ms. C++20 will
define the streaming operator for durations allowing us to write

```cpp
void period(std::chrono::duration<float> seconds)
{
	std::cout << seconds << '\n';
}
```

A `duration` is a class template with two parameters. The first is an
arithmetic type representing the count of ticks. If the type is a floating
point, the duration can represent fractions of a tick. The second parameter is
a `std::ratio`, which is a compile-time constant representing the number of
seconds from one tick to the next. The ratio library provides some convenient
typedefs for several SI units.

For example, if we wanted to define a duration type for the Windows platform
that uses the `DWORD` type to represent the number of milliseconds, we can
define the type `std::chrono::duration<DWORD, std::milli>`.

The chrono library can also catch lossy conversions at compile time, for
example

```cpp
#include <iostream>
#include <chrono>

using namespace std::literals;

void period(std::chrono::duration<unsigned> seconds)
{
	std::cout << seconds.count() << "s\n";
}

int main()
{
	period(3600ms);
}
```

will result in a compiler error notifying us that it can't convert between the
durations. This is because we can't represent 3600ms cleanly as integral
seconds. The chrono library provides the `duration_cast` to explicitly convert
between durations.

```cpp
#include <iostream>
#include <chrono>

using namespace std::literals;

void period(std::chrono::duration<unsigned> seconds)
{
	std::cout << seconds.count() << "s\n";
}

int main()
{
	period(std::chrono::duration_cast<std::chrono::duration<unsigned>>(3600ms));
}
```

This will print 3s. In C++20 the chrono will also add `floor`, `ceil` and
`round` conversion functions for more control on the type of conversion
desired.
