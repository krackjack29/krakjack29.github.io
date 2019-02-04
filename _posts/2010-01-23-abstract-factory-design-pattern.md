---
title: "Abstract Factory – Design Pattern"
date: 2010-01-23 15:27:50 +0530
comments: true
categories: 
   - Design Pattern
tags:
   - designpatterns
   - csharp
layout: single
---

In Abstract factory method there are 4 types of players 
   
1. Abstract Factory – Which is understood by the client and contains a function to create a abstract product.
2. Abstract Product – A theme or an interface understood by the client
3. Concrete Factories – Implementations of the “Abstract Factory”. Which client has no concern off
4. Concrete Products – Implementations of the “Abstract Product”

![Class Diagram](/assets/images/ClassDiagram1.png)

Following code gives an understanding of the abstract factory pattern

```csharp
    
   using System;
   #region Abstract factory
   public abstract class GUIFactory
   {
       public abstract Button CreateButton();
   }
   #endregion

   #region abstract product
   public abstract class Button
   {
       public virtual void Draw()
       {
           Console.WriteLine("I am drawn by ");
       }
   }
   #endregion

   //Client have no info about these concrete products or factories
   #region Concrete Factories
   public class WindowsGUI : GUIFactory
   {
       public override Button CreateButton()
       {
           return new WindowsButton();
       }
   }

   public class MacOsx : GUIFactory
   {
       public override Button CreateButton()
       {
           return new MacOsxButton(); 
       }
   }

   #endregion

   #region Concrete Products
   public class WindowsButton : Button
   {
       public override void Draw()
       {
           base.Draw();
           Console.Write("Windows"); 
       }
   }

   public class MacOsxButton : Button
   {
       public override void Draw()
       {
           base.Draw();
           Console.Write("Mac"); 
       }
   }
   #endregion

   #region Client
   public static class Program
   {
       static void Main()
       {
            //object comes from a config file or created 
            //by some other location
           GUIFactory factory = new WindowsGUI();
           Button but = factory.CreateButton();
           but.Draw(); 
       }
   }
   #endregion
```

In the above quoted example Client has information about the GUIFactory and Button class, while the Implementation itself is hidden from the client.

Advantages of Abstract Pattern

1. The client code has no knowledge whatsoever of the concrete type, not needing to include any header files or class declarations relating to the concrete type. The client code deals only with the abstract type. Objects of a concrete type are indeed created by the factory, but the client code accesses such objects only through their abstract interface.
   
2. Adding new Concrete types is simpler and can be done without re-compiling the client Code.