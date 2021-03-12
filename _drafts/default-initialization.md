---
layout: post
title: "Default Initialization"
---

```cpp
#include <iostream>
#include <vector>

struct Vector2 {
	/*
	 * Why don't we default these to 0 here?
	 * It may be unnecessary, to initialize the values if they're intended to modified
	 */
	float x;
	float y;

	/*
	 * Need to define the default constructor and a constructor
	 * to use emplace_back, I believe this is getting changed in
	 * the future so that this won't be a requirement
	 */
	Vector2() = default;
	Vector2(float x, float y) : x{x}, y{y} { }
};

/*
 * Using this design we can get something close to what we want without
 * introducing an uninitialized constructor, the problem arises when
 * zero is not a good "default" value, for example if we wanted to define
 * a rational class
 */
struct Rational {
	int numerator;
	int denominator;
};

/*
 * It would be nice if we could explicitly construct objects in an initialized
 * state, for example:
 * Vector p = uninitialized;
 */
int main()
{
	Vector2 p; // default-initialized, UB to read from x, y
	Vector2 q{};  // ok, value-initialized, in this case x == 0 && y == 0

	Rational x; // default-initialized, UB to read from numerator, denominator
	Rational y{}; // probably not what we want, numerator == 0 && denominator == 0
	// we would prefer numerator == 0 && denominator == 1
	// This is just a simple example to demonstrate the need for an uninitialized constructor
	// and a default constructor, that can represent some default value

	// Using emplace_back
	std::vector<Vector2> vecs;
	vecs.emplace_back(1, 2);
}
```
