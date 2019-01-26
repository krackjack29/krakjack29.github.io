---
title: .Net Internals
date: 2010/09/22 13:18:00 +530
layout: single
comments: true
categories: 
   - .Net
tags:
   - .net
   - csharp
---
I have been building applications on .Net platforms for about 5 years now and dint understand how it actually happens underneath in the CLR, curse Microsoft for making it so trivial 🙂

I happened to stumble upon this [great article](http://msdn.microsoft.com/en-us/magazine/cc163791.aspx) which gives a clear look at the CLR machinery

Some of the topics covered in this article are

* App Domain – Default domains, system domains
* LoaderHeaps 
* Type Fundamentals 
* ObjectInstance – Memory layout of an object instantiated 
* MethodTable – how methods are resolved and called 
* Base Instance Size 
* Method Slot Table 
* Interface Vtable Map and Interface Map 
* Virtual Dispatch 
* Static Variables

This article also throws some light on a hidden beauty that is shipped with visual studio. ***“Son of Strike”*** , yes the name seems to be some video game but it is a real gem to see what is happening within the CLR.

Follow the steps to use the power of SOS
1. build your project with “Enable unmanaged Code debugging” option in the project properties window.

2. Debug the application.

3. In the immediate window type “!load sos” you are ready to go.

Functions are listed by category. Shortcut names for popular functions are listed in parenthesis.

Type "!help <functionname>" for detailed info on that function.

I would recommend every .Net developer to read the article

Internals – [.NET Framework Internals How the CLR Creates Runtime Objects](http://msdn.microsoft.com/en-us/magazine/cc163791.aspx)

SOS- [http://msdn.microsoft.com/en-us/library/bb190764.aspx](http://msdn.microsoft.com/en-us/library/bb190764.aspx)