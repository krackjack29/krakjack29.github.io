---
title: "Programming Azure TableStorage : Repository Pattern Part -2"
date: 2013/08/09 05:21:00 +530
layout: single
comments: true
categories: 
   - Design Pattern
   - Azure
tags:
   - azure
   - cloud
   - .net
   - csharp
   - designpatterns
---

In my previous blog, i mentioned the basics and the usage of the repository pattern with the TableStorage (only CRUD operations without the ‘R’ reading and querying capabilities).  I did that because we can leverage the [Query Object](http://www.martinfowler.com/eaaCatalog/queryObject.html) pattern to do the reading/querying the storage.

Query object pattern – simply put its representing all the queries as objects

Querying the TableStorage – TableStorage indexes the table data based on PartitionKey and RowKey, but one could still query based on other properties in entity.However querying non key properties would cause full table scan, which will definitely degrade the performance.  So while designing the table one need to extra careful about the usage of partition key and row key.  [More info](http://msdn.microsoft.com/en-us/library/windowsazure/hh508997.aspx).

**ITableReader&lt;T>**  – ITableReader is an interface designed to execute the queries on the table of type T.

```csharp
public interface ITableReader<TTable>
    where TTable : BaseTableEntity,new()
{
    IEnumerable<TTable> FindByFilters(TableFilter<TTable> filters)
}

public abstract class TableFilter<TModel>
    where TModel : BaseTableEntity,new()
{
    protected string PartitionKey { get; set; }

    protected string RowKey { get; set; }

    public abstract Expression<Func<TModel, bool>> Predicate { get; }

    protected abstract string GenerateTableQuery();

    protected virtual string GetPartitionKeyFilterString()
    {
        if (string.IsNullOrWhiteSpace(this.PartitionKey))
            return "1=1";

        return TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, this.PartitionKey);
    }

    protected virtual string GetRowKeyFilterString()
    {
        if (string.IsNullOrWhiteSpace(this.RowKey))
            return "1=1";

        return TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.Equal, this.RowKey);
    }

    public TableQuery<TModel> GenerateQuery()
    {
        var query = new TableQuery<TModel>().Where(this.GenerateTableQuery());
        return query;
    }
}
```

The interface contains one method FindByFilters which takes the abstract class TableFilter which generates the query and the store just executes them and returns back the results.

We could further create 3 more abstract class for the following type of queries 
1. ReadByPartitionKey 
2. ReadByPartitionKeyAndRowKey 
3. ReadByRowKey

```csharp
public abstract class ReadByPartitionKeyAndRowKey<TModel> : TableFilter<TModel>
    where TModel : BaseTableEntity, new()
{

    public ReadByPartitionKeyAndRowKey(string partitionKey, string rowKey)
    {
        this.PartitionKey = RepositoryHelper.AppenedTableNameToKey<TModel>(partitionKey);
        this.RowKey = RepositoryHelper.AppenedTableNameToKey<TModel>(rowKey);
    }

    public override Expression<Func<TModel, bool>> Predicate
    {
        get { return p=> p.PartitionKey == this.PartitionKey && p.RowKey == this.RowKey; }
    }

    protected override string GenerateTableQuery()
    {
        string combinedFilter = TableQuery.CombineFilters(this.GetPartitionKeyFilterString(),
            TableOperators.And, this.GetRowKeyFilterString());

        return combinedFilter;
    }
}

public abstract class ReadByPartitionKey<TModel> : TableFilter<TModel>
    where TModel : BaseTableEntity, new()
{
    public ReadByPartitionKey(string partionKey)
    {
        this.PartitionKey = RepositoryHelper.AppenedTableNameToKey<TModel>(partionKey);
    }

    public override Expression<Func<TModel, bool>> Predicate
    {
        get { return p=> p.PartitionKey == this.PartitionKey; }
    }

    protected override string GenerateTableQuery()
    {
        return this.GetPartitionKeyFilterString();
    }
}

public abstract class ReadByRowKey<TModel> : TableFilter<TModel>
    where TModel : BaseTableEntity, new()
{
    public ReadByRowKey(string rowKey)
    {
        this.RowKey = RepositoryHelper.AppenedTableNameToKey<TModel>(rowKey);
    }

    public override Expression<Func<TModel, bool>> Predicate
    {
        get { return p => p.RowKey == this.RowKey; }
    }

    protected override string GenerateTableQuery()
    {
        return (this.GetRowKeyFilterString());
    }
}
```

few examples of the concrete query classes are

```csharp
public class ContactReadByPartitionKey : ReadByPartitionKey<Contact>
{
    public ContactReadByPartitionKey(string email) : base(email) { }
}

public class ContactReadByTitle : TableFilter<Feed>
{
    string _title;
    public ContactReadByTitle(string title)
    {
        _title = title;
    }

    protected override string GenerateTableQuery()
    {
        return TableQuery.GenerateFilterCondition("Title", QueryComparisons.Equal, this._title);
    }
}
```

In the above example the first class indicates query to read contact whose partitionkey is email, while the second class generates the query to get all the Contacts with title as give value but this query does a full table scan.

You can read about TableQuery.GenerateFilterCondition [here](http://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.table.tablequery_members.aspx)

### Implementing ITableReader<T> in TableStore<T>

TableStore is the repository that we created in the previous blog , this store can implement ITableReader’s FindByFilters method and should be fairly simple affair. Use TableFilter’s GenerateQuery method to generate the query string and pass it as parameter to CloudTable.ExecuteQuery method and voila you are done!!!!.

```csharp
public IEnumerable<TTable> FindByFilters(TableFilter<TTable> filters)
{
    var query = filters.GenerateQuery();
    return this.CloudTable.ExecuteQuery<TTable>(query);
}
```

Happy Coding!!!