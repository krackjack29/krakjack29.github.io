---
title: "SOLID principles of OOAD : Crash Course"
date: 2013/08/23 11:45:00 +530
layout: single
comments: true
categories: 
   - Design Pattern
tags:
   - designpatterns
   - csharp
---

The **SOLID** principles is the basic set of guidelines for any working software design. Its an acronym for 5 rules each represented by a letter in word solid (go figure!!!). 

### S – Single Responsibility Principle 
This principle states that a class should have only one responsibility. It can also be defined as “a class should have one reason to change”.   So when you design a class ask yourself if there is more than one reason the class could change in future, if it does you need to redesign your class or break that class into as many reasons as you found.

### O- Open Closed Principle 
A class must be open for extensions and closed for modifications. If you want to add new functionality or even modify the existing ones you should always extend the class either by polymorphism or inheritance.

### L – Liskov’s Substitution Principle
Each derived class must be substitutable with the base class, in layman’s term derived class could be represented only with its baseclass for functionality. In terms of C# if you find a piece of code which takes instance of base class and applies “as” or “is” keyword its a bad code smell.

### I- Interface Segregation Principle 
This principle kind of goes hand in hand with SRP, the interfaces shouldn’t be fat. For example an animal interface need not contain Walking, all animals donot walk do they?? , so it could be split into two interfaces in themselves and Man can implement both IAnimal and IWalkable interfaces.

### D – Dependency Injection/Inversion Principle 
It states that the classes shouldn’t talk to any concrete implementation, instead should talk only to interfaces or abstract classes, which are exposed either as a parameter or a property. This allows TDD, parallel coding since dependency is only on the interface and not the actual implementation.