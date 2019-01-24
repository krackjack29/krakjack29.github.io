---
title: "One more .Net 4.0 Gem - Lazy&lt;T&gt;"
date: 2010/09/21 16:53:00 +530
layout: single
categories: 
   - .Net
tags:
   - .net
   - csharp
---

There are scenarios when we initialize innumerable objects into the .Net memory before the user code has even used them.

Consider the following code, “Company” object would contain a reference to an array of Contact which is initialized on the class level. When the user instantiates a Company object he would already be allocated this array of Contacts.

```csharp
   public class Company
    {
        private Contacts contacts = new Contacts();
        public void DoSomething()
        {
            ////Doesn't use contacts variable
        }

        public void UseContact()
        {
            //Use the contact variable
            foreach (var item in contacts.GetContact )
            {}
        }
    }
```

Imagine if the Contact would in turn contain array of Order they would be initialized too resulting in memory consumption.

The programmers could write the code to perform lazy initialization of respective , but .Net 4.0 library contains a type which would do the lazy instantiation of the objects. For this topic, the terms lazy initialization and lazy instantiation are synonymous.

**System.Lazy&lt;T&gt;** type in .Net 4.0 allows the objects to be loaded only when its accessed for the first time. In the above example if we were to change the type of contacts to **Lazy&lt;Contacts&gt;** CLR wouldn’t initialize the contacts object unless UseContact() function is accessed.

```csharp
   public class Company
    {
        private Lazy<Contacts> contacts = new Lazy<Contacts>();
        public void DoSomething()
        {
            ////Doesn't use contacts variable
        }

        public void UseContact()
        {
            //Use the contact variable
            foreach (var item in contacts.Value.GetContact )
            {}
        }
    }
```

If you notice that in UseContact() function we are accessing the Contacts through “Value” , it is then that the Contacts object would be initialized.

Lazy&lt;T&gt; type is also thread safe hence we need not be worried about the synchronization while creating the object esp. while implementing the Singleton pattern.

Lazy&lt;T&gt; has 6 overloaded constructors, the default constructor would call the inner type’s (&lt;T&gt;)  constructor, while the user could also pass a delegate of type Func&lt;T&gt; which returns T.

This is truly an hidden gem Microsoft has thrown our way and any enterprise application which needs to load these kind of data should use it.

### References
[MSDN](http://msdn.microsoft.com/en-us/library/dd642331.aspx)

[System.Lazy&lt;T&gt; and the Singleton Design Pattern](http://geekswithblogs.net/BlackRabbitCoder/archive/2010/05/19/c-system.lazylttgt-and-the-singleton-design-pattern.aspx)