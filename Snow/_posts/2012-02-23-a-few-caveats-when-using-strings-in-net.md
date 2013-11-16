---
layout: post
title: A few caveats when using Strings in .Net
categories: .Net
published: true
---
Strings in .Net are class objects and unlike value types they are stored on the heap.

According to msdn: [http://msdn.microsoft.com/en-us/library/ee787088.aspx](http://msdn.microsoft.com/en-us/library/ee787088.aspx)

> Garbage collection occurs when one of the following conditions is true:

> * The system has low physical memory.
> * The memory that is used by allocated objects on the managed heap surpasses an acceptable threshold. This means that a threshold of acceptable memory usage has been exceeded on the managed heap. This threshold is continuously adjusted as the process runs.
> * The GC.Collect  method is called. In almost all cases, you do not have to call this method, because the garbage collector runs continuously. This method is primarily used for unique situations and testing.

Due to the second condition creating a string object can trigger garbage collection, which is costly in terms of performance. As developers we want to keep the string creation to a minimum. Since strings in .Net are immutable (manipulation results in a the creation of a new instance) means that combining two strings "string1" + "string2" results in the creation of three strings and editing a string results in creating a new one with the modification.

I think it's good to have the knowledge about how things work and why even though it's considered an micro optimization.