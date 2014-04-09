---
layout: post
title: A little gem called Hashset
categories: .Net
published: true
---
Hashset is one of those, you know one of those classes, that flies under the radar and avoids detection for a long time.

There is one use-case that hashset is as far as I know, the most appropriate tool for. That is keeping a track of uniquness of value-types. For example keeping track if an Value type identifier has been acted upon or not. Think unique message publishing and handling.

People who haven't heard about hashset (me included) tends to resolve to a Dictionary<TValueType, bool> to keep track of uniqueness. Where TValueType would be the identifier for the thing you're making unique.

If the dictionary already contains the entry, you know you have already taken action upon this identifier.

How I used to solve this use-case with the dictionary approach:
    
	var dictionary = new Dictionary<TIdentifier, bool>();
    if(false == dictionary.ContainsKey(identifier))
	{
		// Mark identifier as handled
		dictionary.Add(identifier, false);

		// Actions to take on identifier not yet handled
	}

The first time I read about HashSet I immediately realized a possible simplification of my dictionary approach to solving the uniqueness use-case.

HashSet has a method bool Add(), which will try to add the identifier and return a boolean wether it already existed or not.

	var hashSet = new HashSet<TIdentifier>();
	if(hashSet.Add(identifier)
	{
		// Actions to take on identifier not yet handled
	}

Even though Dictionary solved the problem, it did add unnecessary code in the form of a negated contains check and an unnecessary adding and store of a value for every entry. Also HashSet is a smaller (functions vise) and therefore more appropriate type for solving this use-case.