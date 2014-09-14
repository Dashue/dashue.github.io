---
layout: post
title:  Typescript: "Only files with a .ts extension are allowed" fix
categories: Typescript, Javascript
---

---excerpt
After encountering this issue multiple times, and each time thinking "This should have a blog post for the next time it's encountered". After what felt like 5 encounters, it was finally time to blog about this.
---end

## Background
The times this issue have been encountered has been when trying to reference a third party javascript library in a typescript file. Upon compilation the following error is presented: *"Incorrect reference: Only files with a .ts extension are allowed"* and typescript will not budge until it is fixed.

## Solution
One solution is to put an empty declaration in the relevant Typescript file, reflecting the functionality of library. This satisfies the typescript compiler and allows the compilation to complete.

###An example: 
Using the library with which this issue was last encountered: *LogEntries*. The basic functionality of LogEntries is comprised of two methods: *init* and *log*. The empty declaration would be the following:

	declare class LE
	{
    	public static init(token: string);
		public static log(message: string);
	}

After that there should be no complaints from the Typescript compiler.