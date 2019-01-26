---
title: Adapter Pattern
date: 2013/09/30 14:36:00 +530
layout: single
comments: true
categories: 
   - Design Pattern
tags:
   - Designpatterns
   - csharp
---

As the name itself suggests “Adapter” pattern allows an interface to ‘adapt’ to another interface. Similar to the universal adapter we all carry when we go outside India helps us to adapt to 110V signal (input interface) to a 220V signal (output interface).

![Adapter](/assets/images/adapter.png)

You can read about the basics of Adapter pattern [here](http://dofactory.com/Patterns/PatternAdapter.aspx#_self2), and following is the walkthrough of the usage in one of the real world scenario.

**Problem**:  Azure table storage requires the entities to be stored within TableStorage to implement ITableEntity interface or TableEntity class, this forms a tight coupling between Azure SDK and the entities that we develop to be stored. What if we decided to move to AWS tomorrow ?

**Solution**: We created our own interface (an abstract class in this example) called it EntityModel

```csharp
public abstract class EntityModel 
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public DateTimeOffset Timestamp { get; set; }
    public EntityModel(string partitionKey, string rowKey)
    {
        this.PartitionKey = partitionKey;
        this.RowKey = rowKey;
    }
    public EntityModel(){}
}
```

Our entity repository which is an internal class expects the entities to be stored to be of type EntityModel instead of TableEntity.

```csharp
public interface IEntityStore<T> : IEntityReadStore<T>
    where T : EntityModel, new()
{
    void Add(T item);
    void Update(T item);
    void Delete(T item);
    void SubmitChanges();
    void DeleteByPartitionKeyAndRowKey(string partitionKey, string rowKey);
}
```

Now we have an input of type “EntityModel” (Adaptee) coming in from the client code and we need to convert that into “TableEntity” (Target) for Azure SDK to do the heavy lifting of storing and retrieving. So we need an adapter .

As per the general definition of Adapter pattern an Adapter must always inherit from the Target and take the adaptee as input. Thats what we did in our CloudTableEntityAdapter class.

```csharp
internal class CloudTableEntityAdapter<T> : ITableEntity
        where T : EntityModel, new() 
```

Its this adapter’s responsiblity to convert the instance of EntityModel to a table-entity.  Tomorrow when we decide to move to AWS or any other NOSql DB we could add a new Adapter and use a factory to create the instances based on the configuration.

Lets see the implementation of ‘Add’ function from the IEntityStore would look like.

```csharp
public void Add(TEntity item)
{
   CloudTableEntityAdapter<TEntity> adapter = new CloudTableEntityAdapter<TEntity>(item);
   this._cloudTable.Execute(TableOperation.Insert(adapter));
}
```

Every function within the respository can utilise the new instance of the adapter and save it to the table storage. The ITableEntity implementation of the adapater would take care of serializing the EntityModel back and forth.

Get the full source code of CloudTableEntityAdapter and EntityModel [here](http://sdrv.ms/1dRDeAd)