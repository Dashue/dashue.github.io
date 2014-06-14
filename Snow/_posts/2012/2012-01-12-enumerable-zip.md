---
layout: post
title: Enumerable.Zip
categories: .Net
published: true
---
I just stumbled upon Enumerable.Zip which is a newly added Linq operator since .NET Framework 4.

Signature is as follows:

    public static IEnumerable<TResult> Zip<TFirst, TSecond, TResult>(
        this IEnumerable<TFirst> first,
		IEnumerable<TSecond> second,
		Func<TFirst, TSecond, TResult> func);

Much like a zipper it "zips" together two IEnumerables applying a specified function over each pair of elements.
I've seen examples which print the greater number from two list of ints, which constructs addresses from four lists of city/street/number/flatnumber and one that prints name and age from name/age lists.

I have as of yet found a good real world example, and am incapable of thinking one up.  
Anyone out there who have made good use of this added functionality?

