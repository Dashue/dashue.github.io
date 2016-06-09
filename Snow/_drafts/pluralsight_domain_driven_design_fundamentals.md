---
layout: post
title: Course Notes: "Domain Driven Design Fundmentals" on Pluralsight
categories: Study, Pluralsight, DDD
published: published
---

---excerpt

---end

From [Domain driven design fundamentals](https://app.pluralsight.com/library/courses/domain-driven-design-fundamentals/table-of-contents) on Pluralsight by Steve Smith and Julie Lerman.

File structure
--------------
Perhaps use screenshot from ddd solution?

Bounded Context
	Infrastructure
		Bounded Context.Data
	Bounded Context.Core
		Interfaces
		Model
			Events
			XyzAggregate
				Class
				Class
				Class
				Class
			ValueObjects
		Services
SharedKernel
	Solution.SharedKernel
		
Bounded contexts
----------------
	split into solution folders

Value Objects
-------------
ValueObject<T>
	https://lostechies.com/jimmybogard/2007/06/25/generic-value-object-equality/
	Immutable (change gives new valueobject)
	Encapsulte related multi value data into single value object (Animal type is species and breed)
	Put functionality here

Entities
--------
	Dont put functionality here, only behaviour
	Don't separate Domain Entity and Stored Entity
	Separate based on Bounded context
	Entity<TId>

SharedKernel
------------
	TagManager.SharedKernel

Builders
--------
	Guard against incorrect values

Domain services
---------------
	Orchestrators for operations involving more than one Entity
	Must take Domain Model Elements
	Stateless
	Only when a different home cannot be found for the func in Entity or Value Object
	Application Layer services (Message sending and receiving), Domain Layer services (Process order, transfer between accounts), Infrastructure Services (send email)
	Should be named using Ubiquitous language (OrderProcessor, ProductFinder)

Invariants
	Enforce consistency throughout the state / data of the entire aggregate root
	Enforced in aggregate root (might introduce new aggregate root)


Refactoring plays a huge role in DDD

Repositories
------------
	Only for aggregate roots
	IAggregateRoot marker interface

Domain Events
-------------
	DomainEvents.Raise(T event) where T: IDomainEvent { DateTime Occurred }
	IHandle<T> where T: IDomainEvent { void Handle(T args) }
	http://udidahan.com/2009/06/14/domain-events-salvation/
	Event Relay Services: To convert from Domain Event (only containing ids, relevant in bounded context) to Application Event e.g UI events(requiring more information not exposed outside of bounded context).

Anti Corruption Layers
----------------------
	Prevent assumptions and design decisions from other system or bounded contexts to bleed into this bounded context
	Cleans up the way you must communicate with other systems
		Facade, Adapter, Custom translation classes or services
	Whatever you need insulate your system from other systems

Aggregates
----------