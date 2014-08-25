---
layout: post
title: Todays Refactoring: Dictionary usage
categories: .Net
published: true
---

---excerpt
Optimizing unoptimized dictionary insertions
---end

Code like the following:

    if (dictionary.ContainsKey(key))
    {
      dictionary[key] = value;  
    }
    else
    {
      dictionary.Add(key, value);  
    }

Can be substituted with the following with the same functionality:

    dictionary[key] = value;

The code will be cleaner, functionality will be the same and a few operations faster :P

