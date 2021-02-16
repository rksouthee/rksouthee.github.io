---
layout: post
title:  "The Game of Stanley Gill"
date:   2019-06-28
categories: invariants algorithms
---

In his manuscript, [On a cultulral
gap](https://www.cs.utexas.edu/users/EWD/transcriptions/EWD09xx/EWD924.html),
Edsger Dijkstra introduces the Game of Stanley Gill. The game extends Euclid's
algorithm to calculate both the `gcd` and `scm` of two positive integers. In
this post we'll prove the correctness of the algorithm.

The game has two inputs, `X` and `Y`, and uses four variables, `x`, `y`, `u`,
and `v`, with initial values: `x = X`, `y = Y`, `u = X`, and `v = Y`. The game
is played while `x != y` and each step we have two conditions to consider. If
`y < x`, decrease `x` by `y` and increase `u` by `v`. If `x < y`, decrease `y`
by `x` and increase `u` by `v`. The following C++ function represents the game.

```cpp
template <typename I>
// requires Integer(I)
std::pair<I, I> gill(const I& X, const I& Y)
{
  // Precondition: X > 0 && Y > 0
  I x = X; I y = Y;
  I u = X; I v = Y;
  while (x != y) {
    if (y < x) {
      x = x - y;
      u = u + v;
    } else {
      y = y - x;
      v = v + u;
    }
  }
  return { (x + y) / I(2),
           (u + v) / I(2) };
}
```

We could return `x` or `y` for the `gcd`, but for the sake of symmetry and
ignoring potential overflow, we'll take the average.

To see that the game correctly computes the result, we'll rely on the following properties:

```
         gcd(a, a) = a
b < a => gcd(a, b) = gcd(a - b, b)
         scm(a, b) = (a * b) / gcd(a, b)
```

we'll also make use of the following invariants: `gcd(X, Y) = gcd(x, y)` and
`2*X*Y = x*v + y*u`.

Before the loop, we have `x = X` and `y = Y`, therefore `gcd(X, Y) = gcd(x, y)`, satisfying our first invariant.
For our second invariant we have, `u = X` and `v = Y`, therefore `x*v + y*u = X*Y + Y*X = 2*X*Y`.

Taking the first condition, `y < x`, and using one of our properties above, we
see that `gcd(X, Y) = gcd(x, y) = gcd(x - y, y)` which satisfies our first
invariant. For the second invariant we have:

```
2*X*Y = x*v + y*u
      = (x - y)*v + y*(u + v)
      = x*v - y*v + y*u + y*v
      = x*v + y*u
```

we see that the right hand side hasn't changed, thereby maintaing our
invariant. The same reasoning applies for the second condition.

Once the loop has completed we have `x = y`, therefore `gcd(X, Y) = gcd(x, y) =
x = y`. Using our second invariant we see that

```
            2*X*Y = x*v + y*u
                  = gcd(X, Y)*v + gcd(X, Y)*u
                  = gcd(X, Y)*(v + u)
2*X*Y / gcd(X, Y) = (v + u)
  X*Y / gcd(X, Y) = (v + u) / 2
        scm(X, Y) = (v + u) / 2
```

therefore we've shown that the Game of Stanley Gill correctly computes both the
gcd and the scm of its inputs. I hope this example was a good demonstration of
the power of invariants. Dijkstra said that it would be impossible to prove the
algorithm correct without the use of invariants.
