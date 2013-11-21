---
layout: post
title: Locking made easy
categories: Uncategorized
published: draft
---
With todays increase in multithreaded solutions locking are more and more often becoming a requirement. Without a clear seperation between business code and locking code it's very easy to get locking wrong and end up with an unnecessary code mess.

The most common issues people I've seen with locking is:

- Having a Shared syncroot for multiple non-related data
 - Which leads to unnecessary and costly synchronization
- Having no visibility where locks are retrieved or set
 - Which leads to hard to read code, and potential deadlocks or unlocked accesses of data 


a

	public class LockBase
	{

		private readonly object _syncRoot;

		public LockBase()
		{
			_syncRoot = new object();
		}
	}