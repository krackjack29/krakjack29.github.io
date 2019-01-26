---
title: "Keyword 'internal' in C#"
date: 2013/09/18 12:38:00 +530
layout: single
comments: true
categories: 
   - .Net
tags:
   - .net
   - csharp
---

"Internal" in C# is used to make the members /methods /classes to be accessible only within that assembly.

Past few days i had been working on a framework to handle multiple NoSQL databases (namely Azure Table Storage, and Simple DB from Amazon) . The problem was any other assembly referring to the framework would also have to refer to the Amazon dlls and Azure dlls, even though i had factories to create them.

Mistake was I had all my classes as public both abstract as well as concrete implementations (yup i am douchebag!!!). 

Then came the "Internal" keyword to the rescue. I put all my concrete implementations as internal classes and methods which were accessing the core Azure apis, while exposed only the interfaces (pure .net) and voila , now when i refer this framework to other assemblies they don’t have to get any other assemblies at all.

You can still have friend assemblies for your unit testing 🙂 read more @ [http://msdn.microsoft.com/en-us/library/0tke9fxk.aspx](http://msdn.microsoft.com/en-us/library/0tke9fxk.aspx)

PS: You definitely need them at run time, but this makes my life so easy.

NOTE: If you think that the classes aren’t really used outside the assembly please leave it unmarked as public as compiler by default makes that assembly as internal.