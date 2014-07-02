---
layout: post
title: Typescript not compiling on build 
categories: Typescript, 
published: true
---

---excerpt
The solution for an issue where typescript was not compiling on build.
---end

## Background
I encountered an issue where typescript was not compiling during the build phase of a project. Typescript was however compiling when files where saved, and although errors when saving wouldn't fail the build, they would show up in the "Error List".

## Solution
It turns out that the order of how you import build targets matters. The import of *Microsoft.TypeScript.targets* must come after *Microsoft.CSharp.targets* and the following resolved the issue.

<br/>

![Solution](/images/2014-07-02.png)