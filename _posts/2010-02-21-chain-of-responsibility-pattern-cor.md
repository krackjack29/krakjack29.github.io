---
title: "Chain of Responsibility pattern (CoR)"
date: 2010-02-21 15:26:22 +530
layout: single
comments: true
categories: 
   - Design Pattern
tags:
   - Designpatterns
   - csharp
---

Recently my colleague enlightened us about a pattern which can be used to hop the inheritance hierarchy.

CoR allows an object to send commands/requests without knowing which object will handle it. The request is sent from one object to other as in a chain and continues till the chain ends (My analogy would be the telephone router, which passes the request from one place to other until a defined connection has been made).

Each object in CoR can handle the request, pass the request to another object or do both.  CoR has 3 main players whose role is mentioned as below

1. **Client** : Requests the handler to perform a job.

2. **Handler**: Abstract/Interface defining the request.

3. **ConcreteHandler**: Implements the handler and does one of the following 
   1. Handles request 
   2. Passes it on to other handlers 
   3. Both

![Uml Diagram](/assets/images/cor.gif)

Now my friend actually tweaked this a little to hop hierarchies of inheritance. Now in place of the successor place a member of type “Type” and in the code just check if the type which has to handle the request is of same type.

The following code piece might be of some help

```csharp
using System;
 
public class BaseClass
{
    public Type PerformerType { get; set; }
 
    public virtual void PerformOprah()
    {
        Console.WriteLine(“Base class is performing opera”);
    }
}
 
public class FirstDerivedClass : BaseClass
{
    public override void PerformOprah()
    {
        if (PerformerType == null || 
            PerformerType == typeof(FirstDerivedClass)  )
        {
            Console.WriteLine(“First derived class is performing opera”);
        }
        else
        {
            base.PerformOprah();
        }
    }
}
 
public class SecondDerivedClass : FirstDerivedClass
{
    public override void PerformOprah()
    {
        if (PerformerType == null || PerformerType == typeof(SecondDerivedClass))
        {
            Console.WriteLine(“Second derived class is performing opera”);
        }
        else
        {
            base.PerformOprah();  
        }
    }
}
 
public class Client
{
    private static void Main()
    {
        SecondDerivedClass testObj = new SecondDerivedClass();
        testObj.PerformerType = typeof(BaseClass);
 
        testObj.PerformOprah();  
    }
}

```
![class diagram](/assets/images/ClassDiagram2.png)

In the above code the hierarchy is as shown in the class diagram. Now if the user or a customizer who wants to get rid of the mid level code, he cant hop the hierarchy and use the BaseClass function directly.

This pattern would be really helpful in a scenario where user would want to use the base class functionality only but not the mid level hierarchy.

## Few drawbacks of this pattern are

### Unhandled requests

Unfortunately, the Chain doesn’t guarantee that every command is handled, which makes the problem worse, since unhandled commands propagate through the full length of the chain, slowing down the application. One way to solve this is by checking if, at the end of the chain, the request has been handled at least once, otherwise we will have to implement handlers for all the possible requests that may appear.

### Broken Chain

Sometimes we could forget to include in the implementation of the handleRequest method the call to the successor, causing a break in the chain. The request isn’t sent forward from the broken link and so it ends up unhandled. A variation of the pattern can be made to send the request to all the handlers by removing the condition from the handler and always calling the successor.

For more information about this pattern please visit

http://www.oodesign.com/chain-of-responsibility-pattern.html

http://en.wikipedia.org/wiki/Chain-of-responsibility_pattern