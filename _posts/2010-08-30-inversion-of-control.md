---
title: Inversion of Control
date: 2010-08-30 17:03:00 +530
layout: single
comments: true
categories: 
   - Design Pattern
tags:
   - .net
   - designpattern
---

Inversion of control is a pattern where the creating and life time of the objects is passed from the client to another layer.

Reasons and scenarios to use this pattern

1. If you want to remove the dependency on a implementation.
2. To unit test each implementation without waiting for the dependent classes.
3. To remove the repetitive code for creating, maintaining the objects.

There are two types of Inversion

1. Dependency Injection
2. Service locator pattern

There are many open source containers which have implemented these patterns few e.g. are Spring.Net, Unity Container of Microsoft etc.

These patterns should be used only for a medium to large type of applications as it would have large overheads.

More information @:

[http://visualstudiomagazine.com/Articles/2010/08/01/Inversion-of-Control-Patterns-for-the-Microsoft-NET-Framework.aspx](http://visualstudiomagazine.com/Articles/2010/08/01/Inversion-of-Control-Patterns-for-the-Microsoft-NET-Framework.aspx)

[http://msdn.microsoft.com/en-us/library/ff647976.aspx](http://msdn.microsoft.com/en-us/library/ff647976.aspx)