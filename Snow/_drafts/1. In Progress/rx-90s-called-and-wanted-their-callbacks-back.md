---
layout: post
title: '90s called and wanted their callbacks back
categories: .Net, Rx
published: draft
---
I recently built a component that needed to make subsribers aware of when things change. There are different ways to accomplish this. We started out by leveraging Action of T.

	// Declaration
	public Action<Payload> ActionCallback;
	
	// Subscription
	ActionCallback = load => { }
	
	// Invokation
	ActionCallback.Invoke(new Payload());

	public class Payload
    {
    }

This turned out to not be the ideal solution for when multiple subscribers exists. It does work by wrapping all subscription calls in the action, but it forces an unnecessary constraint that every subscription needs to be done in the same place.

To solve this problem we changed it to leverage EventHandler. .Net 4.5 removes the EventArgs constraint on EventHandler, unfortunately we were restricted to 4.0 and had to create specific EventArgs classes for the payload. I've seen solutions creating 1 specific EventArgs class for each different type of payload, I find this is a messy approach and therefore created a EventArgs class of T. 

Before .Net 4.5

	// Declaration 
    public EventHandler<EventArgs<Payload>> OldEventHandlerCallback;
    
    // Subscription
    OldEventHandlerCallback += (sender, args) => { };

	// Invokation
    OldEventHandlerCallback.Invoke(this, new EventArgs<Payload>(new Payload()));

	public class EventArgs<T> : EventArgs
    {
        public EventArgs(T payload)
        {
            Payload = payload;
        }

        public T Payload { get; private set; }
    }

 Using .Net 4.5:
	
	// Declaration
	public EventHandler<PayLoad> EventHandlerCallback;

	// Subscription
	EventHandlerCallback += (sender, load) => { };	

	// Invokation 
	EventHandlerCallback.Invoke(this, new Payload());

Observable pattern using Subject:

	// Declaration
	public ISubject<Payload> ObservableSubject = new Subject();

	// Subscription
	ObservableSubject.Subscribe(load => { });
	
	// Invokation
    ObservableSubject.OnNext(new Payload());


dont expose as subject though because.... 
expose as iobservable
	internal ISubject<Payload> subjectPayload = new Subject<Payload>();
    public IObservable<Payload> ObservablePayload;

Subject<int> _Progress;
IObservable<int> Progress {
    get { return _Progress; }
}

Handler goes out of scope? Caller has to make sure to unhook the event or we get memory leak.