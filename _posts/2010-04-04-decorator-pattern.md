---
title: "Decorator Pattern"
date: 2010-04-04 15:24:30 +530
layout: single
comments: true
categories: 
   - Design Pattern
tags:
   - designpatterns
   - csharp
---

Decorator pattern provides a way of attaching a new state and behaviour to the existing objects. Key implementation point for a “Decorator” is that it contains an instance of the component (*“Has-a”*) as well as inherits the component(*“Is-a”*).

Above diagram shows a simple decorator pattern with a component, a operation and a decorator.There could be multiple decorators, multiple components and multiple operations.

The real time example of a decorator pattern would be the photo editor, where the Component would be the photo, Operation would be the the drawing of the photo itself and decorators would be the effect creators such as Frames, Tagging, Glossing, Sharpening etc… Following code shows a simple implementation of the “Decorator” in real world.

```csharp
using System;
namespace Pratap.DesignPatterns.Behaivioural
{
/// <summary>
/// represents the client
/// </summary>
class Program
{
    static void Main(string[] args)
    {
        Photo photo = new Photo();
        PhotoDecorator frameDecorator = new FrameDecorator(photo);
        PhotoDecorator sharpnessDecorator = new SharpnessDecorator(frameDecorator);
        sharpnessDecorator.Draw();   
    }
}
/// <summary>
/// Represents the IComponent in UML diagram
/// </summary>
abstract class IPhoto
{
/// <summary>
/// Represents the operation performed on photo
/// </summary>
    abstract public void Draw();
}
/// <summary>
/// Concrete component class
/// </summary>
class Photo : IPhoto
{
    public override voidDraw()
    {
        Console.WriteLine(“Photo drawn”);
    }
}
/// <summary>
/// Decorator class
/// </summary>
class PhotoDecorator : IPhoto
{
IPhoto photo;
public PhotoDecorator(IPhoto photo)
{
this.photo = photo;
}
public override void Draw()
{
this.photo.Draw();   
}
}
/// <summary>
/// Derived decorator to add the functionality to the operation
/// </summary>
class FrameDecorator:PhotoDecorator
{
public FrameDecorator(IPhoto photo):base(photo)
{
}
public override void Draw()
{
Console.WriteLine(“Added frame”);
base.Draw();
      }
  }
/// <summary>
/// Derived decorator to add the functionality to the operation
/// </summary>
class SharpnessDecorator : PhotoDecorator
{
public SharpnessDecorator(IPhoto photo)  : base(photo)
      {
      }
public override void Draw()
      {
Console.WriteLine(“Sharpness added”);
base.Draw();
      }
   }
}
```

The FrameDecorator and the SharpnessDecorator add new state and functionality to the photo.

Few things that most of us dint know, ***“.Net 3.0 classes have a class known as Decorator (System.Windows.Controls) provides a base class for
elements that apply effects onto or around a single child element, such as Border or Viewbox”***

You can also see decorator pattern in the I/O classes of .Net. The BufferedStream,MemoryStream etc classes are all derived from the Stream class and also contain a private object of Stream.

When can one use decorator pattern? (Can be considered as Pros of the pattern)

* Dynamically attach additional responsibility to the objects
* Make changes to some objects in a class without affecting others
* An existing Component is unavailable for modification
* 
I have started a fascinating journey of design patterns, hope to cover more ground