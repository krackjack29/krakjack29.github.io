---
title: Template Pattern
date: 2010/09/07 18:05:00 +530
layout: single
categories: 
   - Design Pattern
tags:
   - designpatterns
   - csharp
---
In this pattern an abstract class contains a template method which provides a skeleton for the algorithm, one or more steps of the algorithm are made virtual so that the subclasses can have their concrete implementation.

The template method which is implemented in the abstract class should concentrate more on the chronology or the order of the steps needed for the algorithm implementation.

Following is the example of the coffee maker, the template method “MakeCoffee” handles the order with the coffee needs to be made. While the steps to get the coffee and milk are allowed to be implemented by the sub classes.

```csharp
    public void MakeCoffee()
    {
        GetCoffeeBeans();
        GrindCoffeeBeans();
        GetMilk();
    }
```

The following diagram is the class diagram for the above implementation
![Class Diagram](/assets/images/ClassDiagram1.png)

The client would create an instance of either Expresso or Cappucino but the order at which Coffee is made is controlled by the template method (MakeCoffee)

### References
[http://www.dofactory.com/Patterns/PatternTemplate.aspx#_self2](http://www.dofactory.com/Patterns/PatternTemplate.aspx#_self2)

[http://en.wikipedia.org/wiki/Template_pattern](http://en.wikipedia.org/wiki/Template_pattern)