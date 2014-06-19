---
layout: post
title: Why you'd want to place a shared library in its own solution
categories: Software Development
published: true
---

---excerpt
First consumer, second consumer or separate solution, that is the question?
---end

##Background
A situation occurred on work where we decided to refactor duplicated funtionality from two project (A and B) into a new shared library (X) DRYing things up. During this process the question of where to put X came up. I quickly answered "It's own solution" whereupon my colleague countered with the very simple yet valid question "Why" and I realized I didn't have the answer. I know it was really obvious but I had never reflected on the why.

After work I decided to gather my thoughts and write this blog giving a better answer to his valid question.

## Presumptions
* We will use internal Nuget feed for the library.
* X could be placed in either A or B or separate library.

## Consuming the Library
Placing it in its own solution forces all projects consume it the same way, otherwise some projects would use nuget and one could use project reference. 

## Future Changes
If placed in A it would be easy to change according to requirement or quick fix for A and not think about B or any future consumers. Placing it in its own solution forces it to behave more like an independent library and less like a project reference. It makes more sense to open the solution for the library when change is needed than to open an unrelated solution containing the library.

## Unrelated Functionality
Since the functionality is general and not related to neither A nor B, placing it in the same solution just because it's consumed by them is not a valid argument.

## Versioning
The library has different release/deploy cycles than the consumers and using Nuget allows for choosing what version and if/when to upgrade. Different consumers can easily consume different versions of the library. 

## YAGNI
YAGNI was mentioned as an argument against placing X in its own library. I think developers are to fast to throw abbreviations as arguments thinking they are valid without actually considering the situation at hand.

