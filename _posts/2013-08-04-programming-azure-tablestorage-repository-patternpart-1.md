---
title: "Programming Azure TableStorage : Repository Pattern - Part 1"
date: 2013/08/04 00:54:00 +530
layout: single
categories: 
   - Design Patterns
   - Azure
tags:
   - azure
   - cloud
   - .net
   - csharp
   - designpatterns
---
 
>Repository Pattern Mediates between the domain and data mapping layers using a collection-like interface for accessing domain objects. -by Edward Hieatt and Rob Mee

Repository pattern according to me simply put is an in-memory database which abstracts away the actually operations that one needs to do with the physical database (CRUD operations). One could implement their own caching mechanisms in the repository, as well as do transactional operations.

You can learn more about the Repository Pattern [here](http://msdn.microsoft.com/en-us/library/ff649690.aspx) and [here](http://martinfowler.com/eaaCatalog/repository.html).

In my previous blog i mentioned about the class TableEntity and how one should create their own version of BaseTableEntity . In this one i would show how to use Repository pattern to perform Create, Update and Delete operations, i would keep the reading and Query object pattern to the next one.

ITableStore&lt;TTable>
One of the most important and efficient things that .Net provides is Generics, in this situation we could use one TableStore generic class to cater to any entity that client requires.  First we declare a contract for the table store.

```csharp
public interface ITableStore<TTable> : ITableReader<TTable>
    where TTable : BaseTableEntity, new()
{
    void Add(TTable item);
    void Upsert(TTable item);
    void Delete(TTable item);
    void SubmitChanges();
}
```

The generic constraint here restricts the object of the instance to handle only the entities which are of base type BaseTableEntity and has a default constructor used for deserializing the entity when its read back from the storage.

Implementation TableStore&lt;TTable>
In the above contract we have methods which are meant for Create/Update/Delete of the entities while submit changes is the one which will do the heavy loading of actually storing the data in the cloud.

The advantages of doing the above pattern (also called [Unit of Work](http://martinfowler.com/eaaCatalog/unitOfWork.html)) is that we could maintain transactions and also do bulk operations (very helpful when using Cloud Storage)

TableStorage for WindowsAzure has a batch operation class called [TableBatchOperation](http://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.table.tablebatchoperation_members.aspx) which can hold the operations (Add,Delete and Update) as a list and can at a time be sent to execute using the [CloudTable.ExecuteBatch](http://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.table.cloudtable.executebatch.aspx) for synchronous operations or [CloudeTable.BeginExecuteBatch](http://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.table.cloudtable.beginexecutebatch.aspx) for asynchronous.  In my version of the implementation of ITableStore i use asynchronous execution.

Below is the implementation of TableStore

```csharp
public class TableStore<TTable> : ITableStore<TTable>
    where TTable : BaseTableEntity, new()
{
    private readonly string tableName;
    private TableServiceContext _dataServiceContext;
    private int objectsAdded = 0;
    private TableBatchOperation tableBatchOperation;

    internal CloudTable CloudTable
    {
        get { return this._dataServiceContext.ServiceClient.GetTableReference(tableName); }
    }

    public TableStore(ICloudTableContext dataContext)
    {
        this.tableName = RepositoryHelper.GetTableNameFromType<TTable>();
        this._dataServiceContext = new TableServiceContext(dataContext.TableClient);
        this._dataServiceContext.IgnoreResourceNotFoundException = true;
        //Create table if not exists
        this.TryCreateTable();
        this.tableBatchOperation = new TableBatchOperation();
    }

    private void TryCreateTable()
    {
        this.CloudTable.CreateIfNotExists();
    }

    #region ITableRepository<T> Members

    public void Add(TTable item)
    {
        tableBatchOperation.InsertOrReplace(item);
        objectsAdded++;
        //WEAK CODE 
        if (objectsAdded == 10)
        {
            //submit the changes implicitly 
            this.SubmitChanges();
        }
    }

    public void Upsert(TTable item)
    {
        this.tableBatchOperation.InsertOrReplace(item);
        objectsAdded++;
        //WEAK CODE 
        if (objectsAdded == 10)
        {
            //submit the changes implicitly 
            this.SubmitChanges();
        }
    }

    public void Delete(TTable item)
    {
        this.tableBatchOperation.Delete(item);
    }
    public void SubmitChanges()
    {
        try
        {
            if (this.tableBatchOperation.Count > 0)
            {
                TableBatchOperation operationAsBatch = new TableBatchOperation();
                foreach (var item in this.tableBatchOperation)
                {
                    operationAsBatch.Add(item);
                }
                this.SubmitChanges(operationAsBatch);
            }
        }
        finally
        {
            objectsAdded = 0;
            this.tableBatchOperation = new TableBatchOperation();
        }
    }

    private void SubmitChanges(TableBatchOperation batchOperation)
    {
        if (batchOperation.Count > 0)
        {
            var asynResult = this.CloudTable.BeginExecuteBatch(batchOperation,
                this.SubmitChangesCompleted, batchOperation);
        }
    }

    private void SubmitChangesCompleted(IAsyncResult result)
    {
        try
        {
            var resultTable = this.CloudTable.EndExecuteBatch(result);
        }
        catch (StorageException ex)
        {
            int index = -1;
            if (int.TryParse(ex.Message.Split(':')[1], out index))
            {
                var tableBatch = result.AsyncState as TableBatchOperation;
                tableBatch.RemoveAt(index);
                this.SubmitChanges(tableBatch);
            }
        }
        
    }
}
```

Note : The exception handling in the submit completed method, its a crude way but Microsoft hasn’t given us any better way now , i hope they do.

In the next blog, we would go through the reading or querying the TableStorage.