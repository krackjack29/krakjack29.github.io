---
title: Events Vs Delegates
date: 2013/08/22 23:11:00 +530
layout: single
comments: true
categories: 
   - .Net
tags:
   - .net
   - csharp
---

You probably would have heard each interviewer asking this question to you in each and every interview you would have attended. I have asked it many million times myself and listened to a different answer each time.

Even i didn’t know the absolute answer, so decided to dig deep into the topic and below are my findings

Delegates and Events are almost the same, both are function pointers which can be used as members in the classes, entities who have the handle to these members can subscribe and unsubscribe using += and –= operators. Both are by default multi cast.

Following is the code piece for using a delegate and event.

```csharp
namespace Events_And_Delegates
{
    public delegate int CalculatorDelegate(int a, int b);
    public delegate void ErrorOnCalculationDelegate(string exceptionMessage);
    public class Calculator
    {
        public CalculatorDelegate Calculation;
        public event ErrorOnCalculationDelegate ErrorHappened;
        public Calculator()
        {
            this.Calculation += new CalculatorDelegate(this.Divide);
        }
        public int Divide(int a, int b)
        {
            if (b == 0 && this.ErrorHappened != null)
            {
                this.ErrorHappened("Both are 0's");
                return 0;
            }
            return a / b;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            Calculator add = new Calculator();
            //Subscribe to the Delegate
            add.Calculation += new CalculatorDelegate(Program.Subtract);
            //Subscribe to the event
            add.ErrorHappened += new ErrorOnCalculationDelegate(Program.ExceptionHandling);
            //Invoke Delegate to execute all the methods
            add.Calculation(10,5);
            //Invoke Event outside the class
            //add.ErrorOnCalculationDelegate("Test");//Compiler error
        }

        public static void ExceptionHandling(string exceptionMessage)
        {
            System.Console.WriteLine(exceptionMessage);
        }

        public static int Subtract(int a, int b)
        {
            return a - b;
        }
    }
}
```

In the above sample we have a class containing a delegate called CalculatorDelegate and an event called ErrorOnCalculationDelegate. The method Program.Main function which creates an instance of calculator class and subscribes to the delegate and event from the class.

Now the main difference between Events and Delegates is explained in line number 35, **when you try to invoke an event outside the class using the instance handle the compiler throws up error, but a delegate member can be invoked by any object which can access it**.

### How does .Net compiler do this ?

Answer can be found in managed IL code ( use [ILDASM](http://msdn.microsoft.com/en-us/library/f7dy01k1.aspx) tool to decompile the library or app into IL code) the event adds two special methods in the class namely add_{EventName} and remove_{EventName}

![Event Delegates](/assets/images/eventdelegates.jpg)

The ErrorHappened property is made private (one with blue rhombus)

```csharp
.field private class Events_And_Delegates.ErrorOnCalculationDelegate ErrorHappened
```
and another special type called the ErrorHappened (one with green triangle) which becomes the property used by the consumer of the instance.

```csharp
.event Events_And_Delegates.ErrorOnCalculationDelegate ErrorHappened

{

  .addon instance void Events_And_Delegates.Calculator
  ::add_ErrorHappened(class Events_And_Delegates.ErrorOnCalculationDelegate)

  .removeon instance void Events_And_Delegates.Calculator
  ::remove_ErrorHappened(class Events_And_Delegates.ErrorOnCalculationDelegate)

} // end of event Calculator::ErrorHappened
```