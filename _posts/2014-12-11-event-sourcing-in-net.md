---
title: "Event Sourcing in .Net"
date: 2014/12/11 17:53:00 +530
layout: single
comments: true
categories: 
   - Design Pattern
tags:
   - Designpatterns
   - csharp
---

>"The fundamental idea of Event Sourcing is that of ensuring every change to the state of an application is captured in an event object, and that these event objects are themselves stored in the sequence they were applied for the same lifetime as the application state itself."– Martin Fowler

Basically Event Sourcing says anything that happens in the system domain to be captured via events (POCO) and these events are stored (serialized) in a simple data store (document db or even SQL).

### Why Events?

* Events are immutable (already happened in the past), so can be stored in append only data store.
* Events are simple POCOs that describe some action as it occurred, together with associated data it can easily take domain context and meaning e.g. OrderItemAdded, ProductAddedToCart, UserSignedUp etc.

### How to Create an event in .Net?

As mentioned above events is a simple .Net class, similar to the one mentioned below.

```csharp
public abstract class DomainEvent
{
    public Guid Identifier { get; set; }
    public string Source { get; set; }
    public string Target { get; set; }
}

public class OrderItemAddedToCart : DomainEvent
{
    public int ItemId { get; set; }
    public int CartId { get; set; }
    public int Quantity { get; set; }
    public int Price { get; set; }
}
```
Here the OrderItemAddedToCart is the event that is raised when a user adds an item to cart. Isn’t it simple to understand the purpose of the of the class Smile 

### Where would you Raise an event ?

Generally an event should be raised from the business logic or code where you have business rules applied. Event sourcing can also be used as an effective messaging pattern which helps to break down business logic into smaller pieces of code.

```csharp
 interface IProcessEvent<T> where T : DomainEvent
    {
        void Process(T @event);
    }
    public abstract class BaseLogic
    {
        protected void RaiseEvent(DomainEvent @event)
        {
            foreach (var subscribers in GetAllSubscribersForEvent(@event.GetType()))
            {
                subscribers.Process(@event);
            }
            AppendOnlyDataStore.Stream(@event);
        }
    }
    public class CartLogic : BaseLogic
    {
        public void AddOrderItemToCart(OrderItemFromUi order)
        {
            //Do the business logic
            RaiseEvent(new OrderItemAddedToCart
            {
                //Populate values
            });
        }
    }
    public class OrderLogic : BaseLogic,
        IProcessEvent<OrderItemAddedToCart>
    {
        public void Process(OrderItemAddedToCart @event)
        {
            //Do the business logic on the Cart
        }
    }
```
Above implementation of the logic classes are a simple subscriber pattern, the OrderLogic class has subscribed to the OrderItemAddedToCart event , when cartlogic raises the event GetAllSubscribersForEvent method retrieves all the subscribers for that event.

Event processors can be retrieved by reflection or a simple IOCContainer can also be used to register the event processors.

## Advantages of Event Sourcing.

* **Simplification** – Code will be simplified and Single responsibility for each function can be achieved
* **Audit Trail** – Event store acts as an audit trail for each active record and also for the System in whole.
* **Extensibility** – If business requirements change and another action needs to be taken, one could add a new event processor which can be plugged in at runtime.
* **Traceability and Debug ability** – It’s lot easier to find out what action or event processing failed via event store.
* **Fallback** – Its easier to replay all the events from the beginning of time and reconstruct the system when need arises.

## Disadvantages of Event Sourcing

* **Versioning** – Over time when the events change (new properties added or removed) it becomes difficult to maintain.
* **Eventual Consistency** – In above shown example all the events are processed in a synchronous manner, but in most systems it is asynchronous event sourcing hence the completion of all the event processor would be eventually complete.
* **Transactions** – If one of the event handler fails , its very difficult to rollback all the changes that has been made by the other processors to the backing data store.


## References
[http://msdn.microsoft.com/en-us/library/dn589792.aspx](http://msdn.microsoft.com/en-us/library/dn589792.aspx)

[http://codebetter.com/gregyoung/2010/02/20/why-use-event-sourcing/](http://codebetter.com/gregyoung/2010/02/20/why-use-event-sourcing/)

[http://ookami86.github.io/event-sourcing-in-practice/#who-we-are.md](http://ookami86.github.io/event-sourcing-in-practice/#who-we-are.md)

Happy Coding!!!

