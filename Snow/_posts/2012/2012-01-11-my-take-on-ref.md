---
layout: post
title: My take on Ref
categories: .Net, Software Development
published: true
---

---excerpt
A good friend of mine asked me to share my thoughts on the ref keyword
---end

A good friend of mine asked me to share my thoughts on the ref keyword so here goes.

To give it some context: To me a good method is a function taking a variable amount of parameters, making some processing and possibly returning the result. Furthermore the method should do one thing and one thing only and never change input parameter values.

Now the ref keyword allow developers to stray from this "ideal" method by allowing it to change the values of the input parameters. Obfuscating the self-documentation of the method and by this diffusing what the method does and what it's single responsibility is. This creates a required understanding by your fellow colleagues about the intricates of the method, and in turn requires every developer to know how every method in the system is implemented.

The most common reason for using ref parameters is simply that your method is doing to much, rendering you unable to return a single value/collection to account for everything that is being done, ending up with sending and object as input parameter and changing a lot of values. This may be improved by returning part of the data as an out parameter and part of it as the return value if possible.

The reason why I think that using out is better than ref is because out doesn't obfuscate the api and the intention of the method. Using Ref conveys that "This method might(?) change values of the parameter or even the parameter itself" while Out conveys that "This method will change the parameter".

Consider these two implementations of this contrived example:
 
    Example 1: Returning value
	Assert.AreEqual(4, Square(2));
  
	Example 2: Using Ref
	float a = 2;  
	Square(a);  
	Assert.AreEqual(4, a);


I would like to end this post with my oppinion on the "Ref" and "Out" keywords.
> Although there are valid cases when the right solution is to use Ref and Out, from my experince I say you rarely need to use Out, and you need a really good reason for using Ref.

