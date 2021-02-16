---
layout: post
title:  "The Law of Useful Return"
date:   2019-10-27
---

In a [reddit
post](https://www.reddit.com/r/cpp/comments/dhv96a/cppcon_2019_conor_hoekstra_algorithm_intuition/)
regarding [Conor Hoekstra's talk Algorithm
Intuition](https://www.youtube.com/watch?v=pUEnO6SvAMo&feature=youtu.be), a
commenter mentioned an algorithm from the STL that would be useful in solving
one of the problems presented in the talk. The algorithm was `std::search_n`. I
implemented a modified version of the algorithm and noticed that I was
returning more information than the STL's version. This got me thinking about
some of the STL algorithms that seem to throw away useful information.

The following are some of the STL algorithms that throw away information that
it computes or could easily be returned.

* `std::for_each_n` (doesn't return the function)
* `std::search_n` (doesn't return a range)
* `std::copy_n` (throws away the source iterator)
* `std::uninitialized_copy_n` (same as above)

It seems there is a problem when passing counted ranges to algorithms. The
algorithms don't return the position where the iterator ended up after
exhausting the range. It should be noted that there are algorithms in the STL
that do return all the information.

I would like to take this opportunity to mention that bounded ranges aren't
necessarily better than counted ranges. Some algorithms benefit from using a
bounded range, while others benefit from counted ranges, and a lot of the time
the algorithm can work with either equally well.

It would be nice if C++ provided the versions of algorithms that benefited from
counted ranges, such as `std::partition_point`, since it may avoid calling
std::distance if I have the size of the range available. This obviously isn't a
problem when using random access iterators. It can also help make implementing
new algorithms elegant and avoid unnecessary work.

I'm aware introducing lots of different versions of similar algorithms may seem
like bloat, but I'm always reminded of Doug McIlroy's paper, "Mass Produced
Software Components" in which he argues for a large catalogue of components.
