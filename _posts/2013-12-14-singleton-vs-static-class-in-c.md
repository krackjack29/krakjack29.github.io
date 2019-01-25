---
title: "Singleton vs Static Class in C#"
date: 2013/12/14 00:19:00 +530
layout: single
categories: 
   - Design Pattern
tags:
   - Designpatterns
   - csharp
   - .net
---

Singleton pattern is the first pattern one would learn in the GOF patterns. It simply states that only one instance of the class should be created and no more. Below is the simple thread-safe implementation of a Singleton pattern.

```csharp
public class Singleton
{
    private static Singleton _instance;
    private static readonly object _synchronizeContext = new object();

    //Private constructor
    private Singleton(){}
    //Static Instance getter
    public static Singleton Instance
    {
        get
        {
            lock (_synchronizeContext)
            {
                if (_instance == null)
                    _instance = new Singleton();
                return _instance;
            }
        }
    }
    //Other properties to be exposed
}
```

Now the above code would satisfy the design requirements of a Singleton class, but wouldn’t a static class suffice? So I decided to look up and bring some differences between these two things.

1. Singleton classes can have all the capabilities of a normal class i.e 
   * it can inherit from a class or implement an interface
   * can be passed as an parameter
   * can be copied to another reference
   * can be inherited by other classes by making the constructor protected instead of private.

    Static classes cannot have any of those.
2. Singleton classes can be lazy loaded i.e the instantiation happens only when the Instance method is called for the first time.

    Static classes does exactly the same , at least since .Net 3.0 they are loaded for the first time its called.

3. Singleton classes , the coder has to guarantee thread safety

    Static classes are always thread safe (framework takes care of this).

4. The syntactic sugar of not adding static to every property or method written belongs to Singleton classes

    Static classes need to have only static instances.

So those were the obvious differences between a Static class and Singleton class

### When to go with a static class.
* When all the methods are utility methods.
* No state would be stored within the class
* When you don’t expect any kind of interface implementation or substitution.

let me know your feedback about the content through comments.