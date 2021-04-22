---
layout: post
title:  "Improving the Ideal Graph Visualizer for better comprehension of Java's main JIT compiler"
date:   2021-04-22
tags: []
---

Hello, world! This first post describes a tool to visualize the inner workings
of Java's main JIT compiler, and our work to make this tool more useful for
current and potential users.

#### Making sense out of a large, complex JIT compiler

Program comprehension is one of the main challenges in maintaining large and
complex software systems. After a few months trying to find my way around
[C2](http://www.usenix.org/events/jvm01/full_papers/paleczny/paleczny.pdf) (the
main JIT compiler in [OpenJDK](https://openjdk.java.net/), the reference Java
implementation), I can attest to that! Luckily, optimizing compilers like C2 are
often designed around a well-specified representation of the code under
compilation: an [Intermediate
Representation](http://cr.openjdk.java.net/~jrose/draft/code-media.html), or
just "IR". Understanding the structure and main invariants of the IR is often
the gateway to understanding the compiler itself. As Eric Raymond [put
it](http://www.catb.org/~esr/writings/cathedral-bazaar/) (paraphrasing Fred
Brooks): *"Show me your code and conceal your data structures, and I shall
continue to be mystified. Show me your data structures, and I won't usually need
your code; it'll be obvious"*.

#### Ideal Graph Visualizer is our friend

Luckily for those of us who maintain and improve C2, a tool is available to
explore the IR of a program through its different C2 compilation phases: [Ideal
Graph
Visualizer](https://github.com/openjdk/jdk/tree/master/src/utils/IdealGraphVisualizer)
(IGV). IGV is not only "visual", but also interactive, making it possible to
disentangle, with a few mouse clicks, the complex graph that lies at the core of
C2's IR. Created in 2007 by Thomas Wuerthinger as part of his [master's
thesis](https://ssw.jku.at/Research/Papers/Wuerthinger07Master/Wuerthinger07Master.pdf),
IGV is used today as much by C2 newcomers (like me) as by the most experienced
engineers. Time saved by using IGV means more time can be spent improving C2
itself!

#### Improvements in JDK 17

Given the value provided by IGV, we have recently set about improving the tool
so that it can support even better the needs of both experienced and new C2
engineers. Among [other
improvements](https://github.com/openjdk/jdk/pulls?q=IGV+in%3Atitle+author%3Arobcasloz),
we have [fixed the most frequent
crashes](https://github.com/openjdk/jdk/pull/2607), [simplified the build
process](https://github.com/openjdk/jdk/pull/3361), [introduced support for the
latest JDK versions](https://github.com/openjdk/jdk/pull/3361), [made it easier
to search for specific nodes in the IR
graph](https://github.com/openjdk/jdk/pull/2285), and [introduced more intuitive
coloring schemes and default filters to focus on different aspects of the
IR](https://github.com/openjdk/jdk/pull/2499).

![IGV before and after JDK 17 improvements](/assets/before-after-jdk-improvements.png)

{:.image-caption}
*IGV before (left) and after (right) JDK 17 improvements*

These improvements have already made it into [OpenJDK's main
repository](https://github.com/openjdk/jdk), and will be part of [JDK
17](https://openjdk.java.net/projects/jdk/17/). Even thought IGV, as an internal
development tool, is typically not distributed with the virtual machine, we have
made it [very
simple](https://github.com/openjdk/jdk/blob/master/src/utils/IdealGraphVisualizer/README.md#building-and-running)
to build it if you want to give it a try.

#### Future work

In the future, we would like to consolidate the functionality already supported
by IGV as well as extend IGV with new use cases. After talking with a fair
number of C2 engineers from different organizations, it is clear that there is
interest and no shortage of ideas for improving IGV! For example, a recurring
theme is that, while IGV has good support for exploring and tracking the
neighborhood of an IR node through the compilation phases ("bottom-up
exploration"), it could do a better job at presenting the overall structure and
components (e.g. loops) of the program under compilation for top-down
exploration.

#### Getting involved

If you have ever used IGV, it is never too late to tell us about your experience
with the tool and what could be improved in general. Even better, if you want to
get further involved in the improvement work, there is always something to be
done, from reporting issues at the [JDK Bug
System](https://bugs.openjdk.java.net) to actually picking up [IGV improvement
tasks](https://bugs.openjdk.java.net/issues/?jql=labels%20%3D%20c2-igv%20AND%20%28status%20%3D%20open%20OR%20status%20%3D%20new%29%20AND%20assignee%20%3D%20null).
Some of these tasks, [marked with the label
"starter"](https://bugs.openjdk.java.net/issues/?jql=labels%20%3D%20c2-igv%20AND%20labels%20%3D%20starter%20AND%20%28status%20%3D%20open%20OR%20status%20%3D%20new%29%20AND%20assignee%20%3D%20null),
are particularly suitable for newcomers who want to learn and contribute to
OpenJDK: improving IGV is a more fun and less intimidating path towards learning
the internals of a large and complex compiler than hacking the compiler itself.
