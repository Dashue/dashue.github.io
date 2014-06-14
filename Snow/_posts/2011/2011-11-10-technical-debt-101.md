---
layout: post
title: Technical debt 101
categories: Software Development
published: true
---

I would like to start this article with the definition of what technical debt is. Technical debt is taking shortcuts for good or bad, for known or unknown reasons. The technical debt grows exponentially over time because of interest. 

##Interest:

> Every minute spent on not-quite-right code counts as interest on that debt - Wikipedia

The amount of interest is based on a number of things, among others:

* Lack of knowledge - Either from time passing and people forget stuff, or from people changing jobs, leave of absence, death etc.

* Increase in complexity - New functionality will be designed and implemented on top and according to existing technical debt, writing more code to get around code, with more code you need more tests and this code and tests needs review. Hence technical debt will increase technical debt.

##How to spot debt:

There are some signs to look for when you go "debt spotting&" to make it easier to point technical debt.

* The code areas where you as a developer when assigned bugs or to extend this code feel your heart sink, your moral drop e.g spaghetti code, or unnecessary complexity, or if the intent of the code isn't clear enough so it has to use comments or regions as a crutch for relaying what it does

* The average amounts of tasks per sprint decreases

* The number of tests increases at the same time as "average amount of tasks/sprint decreases"

With an agile project methodology you don&#8217;t necessarily have a lower technical debt but because of its support for rapid changes the debt will be highlighted at an earlier stage allowing you to decide if to pay this debt or not.

In my experience test driven development and tests in general protect you from some technical debt because you've had to go through the steps of "red", "green", "refactor"; and thereby paid a little of the debt up front.

##Paying debt:

Like with any debt there is always an enforcer, a collector that will _make_ you pay that debt. With technical debt, this collector is usually time, or new requirements.  

Because of the interest, paying the debt will be more and more expensive as time goes, until you are forced by time to pay, at its most latest stage. This is when its the most expensive to pay the debt because you are most prone to the interests of people with skills have moved on, system have changed, a lot of technical debt has been built and made dependent on underlying technical debt. The debt may even be so large so for you to be able to pay it your feature development will stagnate and may even lead to loss of market pieces.

Often project managers don't allow developers to pay technical debt, or don't prioritise it since its sometimes hard to motivate a sprint that starts with 12 tasks and ends with 12 tasks still to do, with the only thing the developers salary has paid is the debt.

##Types of Technical Debt:

* Bugs are not technical debt until someone decides to not fix them.

* Manage technical debt = planned technical debt

* Unmanaged is the worst, this is only fixed when the pain is felt

##Monitoring Technical Debt:

In order to be able to monitor the debt, you must keep track of it. This involves keeping track of both the managed debt introduced on purpose using shortcuts, quick solutions and the "not-quite-right" solutions, and the unmanaged ones whenever they are encountered and grade them in some way that gives them a relational size difference. My personal view is that for every day that goes by of coding, it's very unlikely that the technical debt remains the same but instead rather if you don't pay some you gain some. Taking time into consideration, just a small daily increase will eventually become very expensive to pay off, leave it too long and a rewrite may end up being your only solution. 


> Technical debt is like an iceberg with only 10 percent visible.


Until next time, keep track of that 10 percent for all your life's worth : )