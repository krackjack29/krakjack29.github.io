---
title: State Pattern
date: 2010/09/14 12:01:00 +530
layout: single
comments: true
categories: 
   - Design Pattern
tags:
   - Designpatterns
   - csharp
---
When we need to implement a state diagram, the first thing we would look to implement is a switch case or a multiple if else statement. This kind of implementation would become difficult to maintain as the number of states increases.

There is a cleaner way of implementing these solution and that’s the state pattern.

State pattern allows the objects to change the behavior when the internal state changes.
![class diagram](/assets/images/state.jpg)

Main players of the pattern are

1. **Context**: Context contains the data and the internal state object and handles all the request made by the client.
2. **State**: Abstract class that lays out the skeleton for the behavior. All the actions should take the Context as the parameter so that they can access the data and change the state.
3. **StateA,StateB**: Concrete implementation of the state model and defines the behavior for a particular state.
   
Real world usage would be the behavior that one would define for different customers (Red,Green and Other).

Following is the simple implementation of the State pattern

```csharp
namespace DesignPatterns.State.Theory
{
    using System;
    public abstract class State
    {
        public abstract void Action(Context context);
    }

    /// <summary>
    /// Concrete implementation of the state
    /// </summary>
    public class StateA : State
    {
        public override void Action(Context context)
        {
            context.ContextData -= 2;
            Console.Write(context.ContextData + " ");
            if (context.ContextData == 0)
            {
                context.State = new StateB();
                Console.WriteLine("State has been changed");
            }
        }

    }

    /// <summary>
    /// Concrete implementation of the State
    /// </summary>
    public class StateB : State
    {
        public override void Action(Context context)
        {
            context.ContextData += 2;
            Console.Write(context.ContextData + " ");

            if (context.ContextData == 10)
            {
                context.State = new StateA();
                Console.WriteLine("State has been changed");
            }
        }
    }

    public class Context
    {
        //Context contains data that could be manipulated by the
        public int ContextData { get; set; }

        private State _state;

        public State State
        {
            get { return _state; }
            set { _state = value;}
        }

        public Context(State state)
        {
            this.State = state;
            ContextData = 10;
        }

        public void Request()
        {
            this.State.Action(this);
        }
    }

    /// <summary>
    /// Client emulation 
    /// </summary>
    class Client
    {
        static void Main(string[] args)
        {
            //intialize the context with default state State A
            Context context = new Context(new StateA());

            for (int i = 0; i < 30; i++)
            {
                context.Request();
            }
            Console.Read();
        }
    }
}
```