---
layout: post
title: Yield!
categories: .Net
published: true
---

Remove unnecessary intermediate collection

var list = new List();

foreach(){
	list.Add();
}

vs

yield


lazy evaluation
one item at a time vs batch, visualize using take.