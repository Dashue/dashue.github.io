---
layout: post
title: Topshelf vs SimpleServices
categories: .Net
published: draft
---
Having gotten tired of having to create three different projects for my windows services, *.Console, *.Service and .*Core, I started looking around for alternative ways of implementing windows services. My requirements are simple:

Debuggable, Testable, Installable

Having found two open source candidates: [Topshelf][0]and [SimpleServices][1]but not any comparison between the two, I decided to write one up.

[0]: https://github.com/Topshelf/Topshelf
[1]: https://github.com/davidwhitney/SimpleServices