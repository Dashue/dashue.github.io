---
layout: post
title: A study in garbage collection
categories: .Net, Study, GC
published: true
---

---excerpt
My notes from Johanathan Worthington's GC talk at "Build Stuff 13"
---end

## Background
For some time I've been meaning to further my understanding of Garbage Collection and the .Net CLR Garbage Collector. The recording of Jonathan Worthington's presentation ["The Secret Lives of Garbage Collectors"] [0] at BuildStuff 13, provided the perfect opportunity. Jonathan is knowledgeable and a fun to listen to, so if you haven't checked this presentation out, I recommend you to do so.

These are my notes, intended for my future self. 

## Terminology

### GC Roots
Active Roots kept track of by JIT. 

- Static variables
- Local variables.

### Reachability Analysis
Produces a slight overestimate of what objects are available

- Start at roots
- Traverse everything reachable from the roots 
	- Local variables
	- Static variables

We know that everything that isn't reachable, cannot be used by the progam and can be collected.

### Write barrier
- Triggered everytime you write a pointer into an object
- Takes the object about to be updated, inspects the pointer its about to write, checks wether the object exist in an older generation.

If the object exists in an older generation

- It's stored into a "remembered set" (objects in a younger generation referenced from an older generation)
- Eventually objects in the younger generation will be moved to an older generation and will then be removed from the remembered set.

*Leveraging a remembered set is cheaper than checking the older generations on every collection*

## Collectors
### Conservative GC
- Walks through the stack and looks at everything.
- False positives
- Performs awfully

### Precise GC
- Knows where everything is, objects and pointers to them
- Makes stack maps when jitting
- Allows to move objects around (alot of GC do this)
    
Two versions of collectors

- Compaction
  Used my most collector 
- Copying

## Implementations

### Generational Collectors
A generationl collector divides objects into generations 2-3 normally. Objects are segregated by age. This works because hypotehtis. 
	
	"The Generational Hyphotesis": 
	The majority of objects in a program, live for a very very short time, or for the whole application.

- Starts by allocating objects into the young generation Gen 0
- If an object survives certain amounts of gc, the object has a longer life time becomes promoted to old generation
	- Allows us to only consider young generations in the majority of garbage collections. 
	- It ignores any pointed to an object in the older generation
	- The young generation stays fairly small, megabytes at each time
	- Full collects happen less often, it then considers all of the generations.

**Comes with a problem:** What happens if the only way an object in the young generation is reachable, is through a pointer from and object in an older generation? If the garbage collecton only considers the young generation, it will think this new object is not referenced, and will free it. This happends when a new item is added to an older list.

This is solved using what is called a *"Write barrier"*, every time an assignment ( = ) is made, objects need to be inspected and any relations in pointers inbetween generations is stored.

**Benefits:** Can use different algorithms for different generations.


## Noteworthy
- Garbage collection can be triggered by anything that can allocate
- GC is moving objects in memory around as your program run
- C# structs are good to know (does not allow stack location)
- Lots ot LOB can hurt (triggering garbage collection)
- Precise lists (prevents resize and performs better)
- Finalizers 
    - might bring the object alive
    - always make object live longer than necessary
- The CLR Has two different Garbage Collectors
    - Client - Concurrent (keeps pause times down)
    - Server - (keeps Throughput rate high)

[0]: http://www.infoq.com/presentations/terminology-garbage-collector 