---
layout: post
title: Locking made easy
categories: .Net
published: true
---

---excerpt
Locking is hard, here is one solution to making it less so.
---end

With todays increase in multithreaded solutions locking are more and more often becoming a requirement. Without a clear seperation between business code and locking code it's very easy to get locking wrong and end up with an tangled mess of code.

The most common issues I've seen with locking solutions are:

- Having a Shared syncroot for multiple non-related data
 - Can lead to unnecessary and costly synchronization
 - Can lead to deadlocks
- Having no visibility where locks are retrieved or set (i.e locks are acquired in multiple locations)
 - Leads to hard to read code
 - Can lead to deadlocks
 - Can lead to unlocked access of data 

So to remedy this, I came up with a simple piece of base that I call LockBase:

	private readonly object _syncRoot;

        public LockBase()
        {
            _syncRoot = new object();
        }

        public void WithLock(Action action)
        {
            lock (_syncRoot)
            {
                action.Invoke();
            }
        }

        public T WithLock<T>(Func<T> action)
        {
            lock (_syncRoot)
            {
                return action.Invoke();
            }
        }
	}

I'm mostly using this to build specific caches. Something along the lines of this:

	public class MyCache : LockBase
	{
		private Dictionary<int, string> _cache = new Dictionary<int, string>();

        public string Get(int key)
        {
            return WithLock(() => _cache[key]);
        }

        public void Set(int key, string value)
        {
            WithLock(() => _cache[key] = value);
        }

        public bool Contains(int key)
        {
            return WithLock(() => _cache.ContainsKey(key));
        }
	}

Hope it helps