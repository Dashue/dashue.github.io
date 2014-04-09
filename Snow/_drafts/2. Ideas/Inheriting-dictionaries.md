---
layout: post
title: Migrating To Sandra.Snow
categories: .Net, Opensource
published: draft
---
### Background
Lately I've come in contact with several implementations of in memory caches.
Looking something like this.


	MyCache
	{
		private Dictionary<TKey, TValue> _cache = new Dictionary<TKey, TValue>();
	}


Improvement
Inherit dictionary
	MyCache : Dictionary<TKey, TValue>
	{
	}


### Drawbacks
C# doesn't allow multi inheritance, you're kind of stuck if you need to inherit something else.
