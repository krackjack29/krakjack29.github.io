---
title: Optional Parameters in C# 4.0
date: 2010/09/15 12:55:00 +530
layout: single
comments: true
categories: 
   - .Net
tags:
   - .net
   - csharp
---

One of the article in Visual Studio Magazine, [5 Traps to Avoid in C#](http://visualstudiomagazine.com/articles/2010/08/31/5-traps-to-avoid-in-csharp.aspx) mentions the pitfalls of using “Optional Parameters”

C# 4.0 re-introduced optional parameters concept which was achieved by the pre-C# 4 era using method overloading.

E.g for Optional Parameters using method overloading

```csharp
    public void GetParameters()
    {
         GetParameters(false);
    }
     
    public void GetParameters(bool flag)
    {
         //Logic goes here
    }
``` 

Assume that GetParamters is part of a library the client who consumes the GetParameters() function would be passing false as the default value. If the default value needs to be changed to true for some reason only the library needs to be compiled.

But the default value for a function using Optional Parameters is fed during compile time, hence the **client consuming the library needs to be recompiled** as well.

More information:

[Named and Optional Arguments](http://msdn.microsoft.com/en-us/library/dd264739.aspx)