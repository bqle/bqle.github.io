---
title: "What happened to Atomic and C++20?"
date: 2022-12-28T14:08:16+07:00
draft: false
---
*In a small benchmark of std::atomic operations, I encountered something bizarre with instantiation of the built-in variables in C++20.*

<!--more-->
## Discovery
Everyone knows atomic operations take a longer time since there is overhead to set up the atomicity. This means our program has to make syscalls to ensure there will be no context switching while we are performing operations on our atomic object. 

However, in that case, one would guess that *instantiating* the atomic variable does not cause any overhead, and so will be comparable to instantiating an ```int```.

Indeed, this is the case up to c++17. A [benchmark](https://github.com/bqle/tlpi/blob/main/benchmarks/bin/atomic_creation_cpp17.o) I made illustrates that they take roughly the same amount of time:
{{<figure src="/blogs/cpp17atomic_creation.png"  width="50%">}}
However, upon recompiling the same benchmark with c++20, instantiation time for an atomic int increase by almost three-fold:
{{<figure src="/blogs/cpp20atomic_creation.png"  width="50%">}}
And note that for versions of c++ before 20, the instantiation times of ```atomic int``` and ```int``` are almost equivalent. What changes occurred with c++ 20 to cause this?

## Godbolt Doesn't Help

A common practice that I've seen C++ developers do is to inspect the compiled binary on [Compiler Explorer](godbolt.org). However, CE does not reveal any differences. Both lines seem to be boiled down to a simple ```mov eax, 0```. I think CE is hiding the constructor instructions of ```std::atomic``` since it's part of the standard library. No clue here.

## Reddit It Is

Sad at my own helplessness and my lack of C++ mentors with whom I can come to, I decided to ask the question on [Reddit](https://www.reddit.com/r/cpp/comments/zxsjs1/stdatomic_and_m1_weird_behavior/). After some back and forth, it seems as though there is a minor difference in the compiler / compiler optimizations. Specifically, when compiled with optimization (greater or equal than -O2), the two instantiations happen at the same speed for cpp20. Any optimization below -O2 still gives discrepant results. 

For now, I have put down my pencil trying to figure out this issue. I am still disastisfied as to:
1. The nature of the optimization being made with -O2
2. The cause of discrepancy between cpp17 and cpp20 *when there is no optimizion*. 

A person on the subreddit also suggested that I should also add more substantial workload for my test to be reliable, but I have increased the loop by 10,000, and the mystery still persists. Maybe I can seek help again once I meet people at HRT. 
