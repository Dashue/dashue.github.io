---
layout: post
title: A little gem called Hashset
categories: .Net
published: true
---

---excerpt
Showing one scenario where Hashset is more appropriate than Dictionary
---end

## Background
Every know and then I stumble upon the Dictionary being used for keeping track of uniqueness. One example usage I recently encountered was keeping identifying weather a message had been published already, alternatively had been received by the subscriber.

An example of how this would look using Dictionary:

	var dictionary = new Dictionary<TKey, bool>();
    if(false == dictionary.ContainsKey(key))
	{
		// Mark identifier as handled
		dictionary.Add(key, true);

		// Do stuff
	}

In this case the Dictionary allows for an unnecessary extra dimension in the form of a **Value**. If the key exists, we could assume it has been handled.

## HashSet
There there is an even more appropriate data structure, and a surprisingly large group of people haven't heard about it, called HashSet. A HashSet is like a Dictionary without the Value dimension, just the key/identifier portion.

HashSet has a method with the signature of bool Add(TKey), which will try to add the identifier and return a boolean wether it already existed or not.

An example of how this would look:

	var hashSet = new HashSet<TIdentifier>();
	if(hashSet.Add(identifier)
	{
		// Actions to take on identifier not yet handled
	}


## Summary
Even though Dictionary solved the problem, it added unnecessary code and computation in the form of a check for existence and an unnecessary memory comsumption for storing an extra unused value for every entry. Whereas a HashSet saves us both memory and computation. Using the right type for the problem at hand is always a good thing and becomes even more important and possibly even critical as the number of elements increase.
