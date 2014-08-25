---
layout: post
title: Brainfart when sorting Lists
categories: .Net
published: true
---

---excerpt
Optimizing unoptimized sorting of a list.
---end

Just encountered this piece of code today:
    
    Array.Sort(myList.ToArray());
    foreach(var i in myList)
    { // Do something with i }
    
My first though was “Wait what?” Is there some hidden functionality of List.ToArray() that I have missed which only is passed onto jedi masters? Does this way of sorting perform better than just List.Sort?

I had to investigate, so I wrote up this little code:

    var list = new List<int&gt; {9, 4, 7, 1, 2};
    
    Array.Sort(list.ToArray());
    foreach (var i in list)
    {
       Console.WriteLine(i);
    }
    
    Console.ReadKey();
    
Lo and behold the printed sequence is: 9 4 7 1 2, hence we are creating an array, sorting it then throwing it away, only to use our list in the foreach.
    
List.Sort uses Array.Sort internally so I would dare go out on a limb and say that speed for sorting should be the same