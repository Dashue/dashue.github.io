---
layout: post
title: .Net Memory Management 101 
categories: .Net, Memory, GC
---

---excerpt
A brieft explanation of memory management and garbage collection in .net
---end

## Background
I recently viewed a video about a memory profiling product from Jetbrains: dotmemory 4. To get everyone on the same level, Maarten Balliauw provided a brief explanation to memory management and garbage collection in .Net.

I thought the explanation was clear and spot on and this post is intended to recapture it for future references.

With this only being the notes from his talk, I recommend the reader to view the video for the full explanation.

The video can be found here ["DotMemory 4: What's new?"] [0]

## Content

### Memory Allocation
+ .Net runtime reserves a region of address space for every new process called the *"managed heap"*
+ Objects are allocated in the heap
+ Allocating memory is fast, it's just adding a pointer
+ Some unmanaged memory is also consumed and will not be collected *(.NET CLR, Dynamic libraries, Graphics buffer and more)*

### Memory Release aka "Garbage Collection" (GC)
+ Releases objects no longer in use by examining application roots
+ Builds a graph that contains all objects that are reachable from these roots
+ Removes the object from the heap (releases memory) if it is not reachable from the graph
+ Compacts reachable objects in memory

### Generations
+ Managed heap is divided into three segments called *"generations"* or *"gen"*
+ New objects go into Gen 0
+ Gen 0: Short-lived objects (e.g Local variables)
+ Gen 1: In-between objects
+ Gen 2: Long-lived objects (e.g App main, form, static)

### Memory release implementation
+ When Gen 0 is full
    + Perform GC on Gen 0
    + Promote reachable objects to Gen 1
    + This is typically pretty fast
+ When Gen 1 is full
    + Perform GC on Gen 1 and Gen 0
    + Promoted rechable object to Gen 2
+ When Gen 2 is full
    + Perform "*Full GC*" (2, 1, 0)
    + Throws *OutOfMemoryException* if not enough memory exists for new allocations

*Full GC* has a performance impact, since all objects in the managed heap are verified, not just objects reachable from graph.

### Large Object Heap "LOH"
+ Separate segment within the managed heap
+ Large objects >85kb
+ Only collected during *Full GC*

*LOH* is not compacted and becomes fragmented over time. This fragmentation will cause *OutOfMemoryException* if there is not enough free space for new objects.

[0]: http://youtu.be/1lqfxl8AJR0