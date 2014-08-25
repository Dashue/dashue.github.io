---
layout: post
title: Implicit Operators
categories: .Net
published: true
---

---excerpt
Implicit operators can be used for a lot of differnet things. One usage is explained in this post: Implicit mapping between types.
---end

## Introduction
Mapping occur in many places in applications, mostly in boundaries between layers. Mapping one type to another, be it server to client DTOs or something else, there are numerous ways of accomplishing it. 

Some use a static Mapper factory, some use tools like AutoMapper, my preferred way of doing it is with the use of Implicit Operators.

Like so:

	var domainModel = new DomainModel
	{
		Firstname = "Firstname",
		Lastname = "Lastname"
	};
     
    ViewModel viewModel = domainModel;
     
What allows us to do this is something called an "Implicit Operator". Which are defined like below.
 
    private class ViewModel
    {
        public string Name { get; set; }
     
    	public static implicit operator ViewModel(DomainModel model)
    	{
    		return new ViewModel
    		{
    			Name = model.Firstname + " " + model.Lastname
    		};
    	}
    }
 
    private class DomainModel
    {
        public string Firstname { get; set; }
        public string Lastname { get; set; }
    }