---
layout: post
title:  "Balanced Reduction"
date:   2019-07-26
categories: c++ algorithms
---

In Elements of Programming, Alexander Stepanov and Paul McJones describe an
algorithm that takes a sequence and a partially associative operation and
reduces the sequence to a single value. The algorithm works by maintaining a
counter and combining elements of the same "weight". This reduction algorithm
can help turn a recursive process into an iterative process. The algorithm also
works when the size of the range is not known, that is, the algorithm could
take values from an input stream.

The first component we'll need is `add_to_counter`. As the name implies, it
will add a value to the current state of the counter. It does this by iterating
through the counter, if at the current position the value is "zero" we'll
insert the value there and we're done. If the value at the current position is
not "zero" we combine the value we're trying to insert with the value at the
current position, set the value at the current position to "zero" and move to
the next element in the counter. If we've reached the end of the counter
without inserting our new value, we'll return the value that was accumulated
along the way.

```cpp
template <typename I, typename Op, typename T>
T add_to_counter(I first, I last, Op op, T x, const T& zero)
{
  while (first != last) {
    if (*first == zero) {
      *first = x;
      return zero;
    }
    x = op(*first, x);
    *first++ = zero;
  }
  return x;
}
```

The order of operands to the operation is important. We want elements that were
inserted into the counter first to be the first operand.

For example, we'll take the sequence [11, 10, 19, 13, 18, 4, 15], add these
values to our counter, which we'll be an array of size 3 all initialized to
zero. Our operation will be addition and our "zero" will be 0, the additive
identity.


| Value | counter[0] | counter[1] | counter[2] |
| ----- | ---------- | ---------- | ---------- |
| -	| 0	| 0	| 0 |
| 11	| 11	| 0	| 0 |
| 10	| 0	| 21	| 0 |
| 19	| 19	| 21	| 0 |
| 13	| 0	| 0	| 53 |
| 18	| 18	| 0	| 53 |
| 4	| 0	| 22	| 53 |
| 15	| 15	| 22	| 53 |

If we imagine the state of the counter as an n-bit binary number and view
non-zero values as a 1, then the counter represents the number of elements that
have been inserted into the counter.

With our values added to the counter we'll now want to reduce the counter.

```cpp
template <typename I, typename Op, typename T>
T reduce_counter(I first, I last, Op op, const T& zero)
{
    while (true) {
        if (first == last) return zero;
        if (*first != zero) break;
        ++first;
    }

    T x = *first++;
    while (first != last) {
        if (*first != zero)
            x = op(*first, x);
        ++first;
    }
    return x;
}
```

Once again we have respect associativity, therefore the order of the operands
to the operation is important.

With this machine we can implement reverse on forward iterators where our
operation is rotate, sort where the operation is merge, a binomial queue where
the operation merges two binomial trees of the same weight. We could also use
this machine to add floating-point numbers to reduce error accumulation.

[Here](https://github.com/rksouthee/balanced-reduction) is a link to some
solutions to exercises from the book, Elements of Programming, using balanced
reduction.

[Here](https://github.com/rksouthee/scratch/blob/master/binomial_queue.cpp) is
a link to some code using balanced reduction to implement a binomial queue.
