---
title: "What happened to Atomic and C++20?"
date: 2022-12-28T14:08:16+07:00
draft: false
---
*In a small benchmark of std::atomic operations, I encountered something bizarre with instantiation of the built-in variables.*

<!--more-->
## Discovery
Everyone knows atomic operations take a longer time since there is overhead to set up the atomicity. This means our program has to make syscalls to ensure there will be no context switching while we are performing operations on our atomic object. 

However, in that case, one would guess that *instantiating* the atomic variable does not cause any overhead, and so will be comparable to instantiating an ```int```.

Indeed, this is the case up to c++17. A [benchmark](https://github.com/bqle/tlpi/blob/main/benchmarks/bin/atomic_creation_cpp17.o) I made illustrates that they take roughly the same amount of time:
{{<figure src="/blogs/cpp17atomic_creation.png"  width="50%">}}
However, upon recompiling the same benchmark with c++20, instantiation time for an atomic int increase by almost three-fold:
{{<figure src="/blogs/cpp20atomic_creation.png"  width="50%">}}
And note that for versions of c++ before 20, the instantiation times of ```atomic int``` and ```int``` are almost equivalent. What changes occurred with c++ 20 to cause this?

## Godbolt Doesn't Explain
