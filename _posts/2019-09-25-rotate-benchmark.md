---
layout: post
title:  "Rotate Benchmark"
date:   2019-09-25
categories: c++ algorithms
---

There are three algorithms to implement rotation, Gries-Mills, three-reverse,
and gcd cycles. Each of these algorithms have different iterator requirements.
Gries-Mills requires ForwardIterators, three-reverse requires
BidirectionalIterators, and gcd cycles requires RandomAccessIterators. It is
interesting to note that the major standard implementations, libstdc++, libc++
and Microsoft's STL each use a different algorithm for RandomAccessIterators.

I wrote a benchmark comparing the three algorithms. For the benchmark I use
three different sizes for the type, 4, 8, and 24. Determining the rotation
amount, I use three values 30%, 60% and 90%. During benchmarking I noticed that
the algorithm for ForwardIterator was performing quite well, so I decided to
look into optimizing the algorithm for RandomAccessIterators. I had thought I
discovered the optimization but during my research of the standard
implementations I found out that libstdc++ has this implementation (they also
optimize some special cases). I also discovered that Microsoft's implementation
which uses three-reverse, is very fast, this is due to their implementation of
std::reverse which use vector instructions when possible. Finally libc++'s
implementation for RandomAccessIterators uses gcd cycles, this turns out not to
be practical compared to the other algorithms even though it performs the
fewest number of operations. This is most likely due to the long jumps that the
algorithm requires, thereby not being as cache friendly as the other two
algorithms.

The following tables are the results from my benchmarks. The values are the
time (nanoseconds) taken per element.

### Element Size: 4, Rotation Amount: 30%

| size |    forward | optimized |       bidirectional |   random access |
| ---- | ---------- | --------- | ------------------- | --------------- |
| 1024 |    3 |       3 |       3 |       3 |
| 2048 |    3 |       3 |       3 |       3 |
| 4096 |    3 |       2 |       3 |       3 |
| 8192 |    3 |       2 |       3 |       3 |
| 16384 |   3 |       3 |       3 |       3 |
| 32768 |   3 |       2 |       3 |       3 |
| 65536 |   3 |       3 |       3 |       3 |
| 131072 |  3 |       2 |       3 |       3 |
| 262144 |  3 |       2 |       3 |       3 |
| 524288 |  3 |       3 |       3 |       3 |
| 1048576 | 3 |       3 |       3 |       4 |
| 2097152 | 3 |       3 |       3 |       4 |
| 4194304 | 3 |       3 |       3 |       4 |
| 8388608 | 4 |       3 |       3 |       5 |
| 16777216 |        4 |       3 |       3 |       7 |

### Element Size: 4, Rotation Amount: 60%

| size |    forward | optimized |       bidirectional |   random access |
| ---- | ---------- | --------- | ------------------- | --------------- |
| 1024 |    4 |       3 |       3 |       3 |
| 2048 |    3 |       2 |       3 |       3 |
| 4096 |    3 |       2 |       3 |       3 |
| 8192 |    3 |       3 |       3 |       3 |
| 16384 |   3 |       2 |       3 |       3 |
| 32768 |   3 |       2 |       3 |       3 |
| 65536 |   3 |       2 |       3 |       3 |
| 131072 |  3 |       3 |       3 |       3 |
| 262144 |  3 |       2 |       3 |       3 |
| 524288 |  3 |       2 |       3 |       3 |
| 1048576 | 3 |       3 |       3 |       3 |
| 2097152 | 3 |       3 |       3 |       3 |
| 4194304 | 3 |       3 |       3 |       3 |
| 8388608 | 3 |       3 |       3 |       4 |
| 16777216 |        3 |       3 |       3 |       4 |

### Element Size: 4, Rotation Amount: 90%

| size |    forward | optimized |       bidirectional |   random access |
| ---- | ---------- | --------- | ------------------- | --------------- |
| 1024 |    3 |       2 |       3 |       3 |
| 2048 |    3 |       2 |       3 |       3 |
| 4096 |    3 |       2 |       3 |       3 |
| 8192 |    3 |       2 |       3 |       3 |
| 16384 |   3 |       2 |       3 |       3 |
| 32768 |   3 |       2 |       3 |       3 |
| 65536 |   3 |       2 |       3 |       3 |
| 131072 |  3 |       3 |       3 |       4 |
| 262144 |  3 |       2 |       3 |       3 |
| 524288 |  3 |       2 |       3 |       3 |
| 1048576 | 3 |       3 |       3 |       3 |
| 2097152 | 3 |       3 |       3 |       5 |
| 4194304 | 3 |       3 |       3 |       6 |
| 8388608 | 4 |       3 |       3 |       4 |
| 16777216 |        4 |       3 |       3 |       4 |

### Element Size: 8, Rotation Amount: 30%

| size |    forward | optimized |       bidirectional |   random access |
| ---- | ---------- | --------- | ------------------- | --------------- |
| 1024 |    3 |       2 |       2 |       3 |
| 2048 |    2 |       2 |       2 |       3 |
| 4096 |    2 |       2 |       2 |       3 |
| 8192 |    2 |       2 |       2 |       3 |
| 16384 |   2 |       2 |       2 |       3 |
| 32768 |   2 |       2 |       2 |       3 |
| 65536 |   2 |       2 |       2 |       4 |
| 131072 |  2 |       2 |       2 |       4 |
| 262144 |  2 |       2 |       2 |       3 |
| 524288 |  2 |       2 |       3 |       3 |
| 1048576 | 4 |       4 |       4 |       9 |
| 2097152 | 5 |       4 |       5 |       10 |
| 4194304 | 5 |       4 |       5 |       5 |
| 8388608 | 6 |       5 |       5 |       8 |
| 16777216 |        7 |       5 |       5 |       13 |

### Element Size: 8, Rotation Amount: 60%

| size |    forward | optimized |       bidirectional |   random access |
| ---- | ---------- | --------- | ------------------- | --------------- |
| 1024 |    3 |       2 |       2 |       3 |
| 2048 |    2 |       2 |       2 |       3 |
| 4096 |    2 |       2 |       2 |       2 |
| 8192 |    2 |       2 |       2 |       3 |
| 16384 |   2 |       2 |       2 |       3 |
| 32768 |   2 |       2 |       2 |       3 |
| 65536 |   2 |       2 |       2 |       3 |
| 131072 |  2 |       2 |       2 |       3 |
| 262144 |  2 |       2 |       2 |       3 |
| 524288 |  3 |       2 |       2 |       3 |
| 1048576 | 4 |       3 |       4 |       5 |
| 2097152 | 5 |       4 |       5 |       4 |
| 4194304 | 5 |       4 |       5 |       5 |
| 8388608 | 5 |       4 |       5 |       7 |
| 16777216 |        7 |       4 |       5 |       6 |

### Element Size: 8, Rotation Amount: 90%

| size |    forward | optimized |       bidirectional |   random access |
| ---- | ---------- | --------- | ------------------- | --------------- |
| 1024 |    3 |       2 |       2 |       3 |
| 2048 |    2 |       2 |       2 |       3 |
| 4096 |    2 |       2 |       2 |       2 |
| 8192 |    2 |       2 |       2 |       3 |
| 16384 |   2 |       2 |       2 |       3 |
| 32768 |   2 |       2 |       2 |       3 |
| 65536 |   2 |       2 |       2 |       3 |
| 131072 |  2 |       2 |       2 |       4 |
| 262144 |  2 |       2 |       2 |       4 |
| 524288 |  3 |       2 |       2 |       3 |
| 1048576 | 4 |       3 |       4 |       6 |
| 2097152 | 4 |       4 |       5 |       13 |
| 4194304 | 4 |       4 |       5 |       10 |
| 8388608 | 6 |       5 |       5 |       6 |
| 16777216 |        7 |       5 |       5 |       8 |

### Element Size: 24, Rotation Amount: 30%

| size |    forward | optimized |       bidirectional |   random access |
| ---- | ---------- | --------- | ------------------- | --------------- |
| 1024 |    4 |       3 |       3 |       4 |
| 2048 |    5 |       4 |       4 |       4 |
| 4096 |    5 |       4 |       4 |       4 |
| 8192 |    5 |       5 |       5 |       5 |
| 16384 |   6 |       5 |       5 |       5 |
| 32768 |   6 |       5 |       5 |       6 |
| 65536 |   6 |       5 |       6 |       7 |
| 131072 |  6 |       5 |       6 |       7 |
| 262144 |  9 |       9 |       8 |       7 |
| 524288 |  13 |      12 |      12 |      20 |
| 1048576 | 13 |      12 |      14 |      25 |
| 2097152 | 14 |      13 |      14 |      24 |
| 4194304 | 15 |      14 |      14 |      14 |
| 8388608 | 17 |      14 |      15 |      21 |
| 16777216 |        20 |      14 |      14 |      26 |

### Element Size: 24, Rotation Amount: 60%

| size |    forward | optimized |       bidirectional |   random access |
| ---- | ---------- | --------- | ------------------- | --------------- |
| 1024 |    6 |       3 |       3 |       3 |
| 2048 |    5 |       4 |       4 |       4 |
| 4096 |    5 |       4 |       4 |       4 |
| 8192 |    6 |       4 |       5 |       4 |
| 16384 |   6 |       5 |       5 |       5 |
| 32768 |   6 |       5 |       5 |       7 |
| 65536 |   6 |       5 |       6 |       7 |
| 131072 |  6 |       5 |       6 |       4 |
| 262144 |  9 |       8 |       8 |       7 |
| 524288 |  13 |      11 |      13 |      19 |
| 1048576 | 13 |      12 |      14 |      18 |
| 2097152 | 15 |      13 |      14 |      10 |
| 4194304 | 15 |      14 |      15 |      14 |
| 8388608 | 17 |      13 |      14 |      20 |
| 16777216 |        19 |      13 |      14 |      18 |

### Element Size: 24, Rotation Amount: 90%

| size |    forward | optimized |       bidirectional |   random access |
| ---- | ---------- | --------- | ------------------- | --------------- |
| 1024 |    6 |       3 |       4 |       3 |
| 2048 |    5 |       4 |       4 |       4 |
| 4096 |    5 |       4 |       4 |       4 |
| 8192 |    5 |       5 |       5 |       5 |
| 16384 |   6 |       5 |       6 |       5 |
| 32768 |   6 |       5 |       5 |       5 |
| 65536 |   6 |       5 |       5 |       7 |
| 131072 |  6 |       5 |       6 |       7 |
| 262144 |  10 |      8 |       8 |       10 |
| 524288 |  12 |      10 |      13 |      13 |
| 1048576 | 12 |      10 |      14 |      20 |
| 2097152 | 14 |      13 |      14 |      26 |
| 4194304 | 16 |      14 |      14 |      24 |
| 8388608 | 17 |      14 |      14 |      14 |
| 16777216 |        20 |      14 |      14 |      21 |

As you can see, the the three-reverse algorithm is quite stable. The optimized
forward iterator version is usually the fastest algorithm. The algorithm using
gcd cycles (used by libc++) usually performs the worst.

## Conclusions

The benchmark should include a version using vectorized reverse (for types
where SIMD can be used). I know from previous benchmarks I've done the
algorithm is the fastest. An adaptive version of the optimized Gries-Mills
should also be benchmarked.

Based on the results of the benchmark, I believe an ideal rotate algorithm
would dispatch to using three-reverse when vectorization is possible, otherwise
the optimized algorithm for forward iterator seems to provide the best
performance.
