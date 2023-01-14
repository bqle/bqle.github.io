---
title: "Do Not Move Built Ins"
date: 2023-01-13T20:47:52-05:00
draft: false
---
*Sanity check for the adage "Do not move built-ins"*

<!--more-->
## Moving vs Copying Built-Ins
Equipped with the ability to benchmark my code - all thanks to [Google Benchmarks](https://github.com/google/benchmark), I now have the desire to test the advice given to me by CPP veterans. I know I should not be bull-headed and only believe things when I see them, but it is also intrinsically satisfying for me to witness things for myself. At a certain point in one's education journey, one has been taking the words of experts for so long, one simply wishes they could witness evidence with their own eyes. After all, isn't this aprt of the joy of computer science? That we could test things for ourselves with only a laptop and nothing more? 

Okay, back to business. Following are the results of my benchmarks for whether one should move or copy basic built-ins like int. I have compared moving & copying for both a class that contains only one simple built-in and an instantiation of the built-in itself:
{{<figure src="/blogs/do-not-move-built-ins.png" width="50%">}}
As one can see, an the instantiation of the class of only one built-in takes approximately 4% less time than moving it. Similarly, copy a built-in takes approximately 5% less than than moving it. Thus, in our example, we have shown that not only for built-ins but for classes that contain exactly one built-in, copying the object is slightly faster than moving the object. 
Obviously, as shown in the last benchmark, passing by reference is faster than all other ways of passing arguments.

## Again, Compiler Weirdness
One other interesting (granted it could also totally be due to my writing benchmarks wrong) is that when I up the number of built-ins within a class to 8, the time to copy drop precipitously by almost a third. Yet, when I up it once more to contain 9 built-ins, the time to copy suddenly skyrockets (almost quadruble) to the level expected as shown below: 
{{<figure src="/blogs/more-built-ins.png" width="50%">}}
At this point, I will blame all my benchmarks weirdness on the compiler, but I truly am at odds because it seems like every benchmarking experiment I conduct seems to produce some funkiness. 
Perhaps it is a task for future me to dive into Compiler Explorer to understand what is going on. However, for tonight, I willingly accept my ignorance on this matter, and be glad that I have *somewhat* confirmed the veterans' advice for beginners like me.