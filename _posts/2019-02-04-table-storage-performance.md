---
title: 'Azure Table Storage performance'
layout: single
comments: true
date : 2019/02/03
categories:
    - azure
tags:
    - azure
    - cloud
    - functions
    - appinsights
    - performance
toc: true
---

[Azure table storage](https://docs.microsoft.com/en-in/azure/storage/tables/table-storage-overview) is one of the cheapest No-SQL (Key value store) datastore amongst other services. Table storage can be used for multiple scenarios such as configuration store, diagnostics logs, WAD logs etc.

I had written some blogs earlier for writing and reading into table storage using the repository pattern.
* [Part 1]({% post_url 2013-08-04-programming-azure-tablestorage-repository-patternpart-1 %})
* [Part 2]({% post_url 2013-08-09-programming-azure-tablestorage-repository-pattern-part-2 %})

We had a scenario where we were to import about 4 million records each month of data from a CSV file into table storage with a partition key as the id and at most 6-10 records per partition key. The question here was how would table storage fair when it has about a million partition keys.

So I decided to do an experiment to understand the write and read performance of a fully loaded table storage.

## Experiment

I had integrated application insights with the azure function which would give the duration taken by a [dependency](https://docs.microsoft.com/en-us/azure/azure-monitor/app/asp-net-dependencies) (HTTP requests, CosmoDb, TableStorage, Blob etc). I would recommend each application that runs on Azure should be integrated with application insights to understand the bottlenecks, performance statistics, create alerts etc. You can read more about application insights [here](https://docs.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview).

### Insert Function and Statistics

I wrote an azure function with a timer trigger of 30secs, with each execution it would insert a batch of 1-10 records for a partition key (Guid). Its a simple entity with no properties just the rowkey and partitionkey.

```csharp
public class Entity : TableEntity
{
    public Entity()
    {}
    public Entity(string partitionKey)
    {
        PartitionKey = partitionKey;
        RowKey = Guid.NewGuid().ToString();
    }
}
```

This function ran for about a week and generated about half a million records.

I left this function to run for about a week, it currently has around 600K records with at least 100K unique partition keys. Using [Appinsight analytics](https://docs.microsoft.com/en-us/azure/azure-monitor/app/analytics) below is the timechart for average insertion time in 10 min window for the last 7 days.

![Timechart for insertion](/assets/images/tspinsertgraph.PNG)

If you notice the graph above the insertions is mostly consistent around **50ms** range except for few anamolies (This happens to all services within Azure, even Microsoft suggests to build in resliency as part of code). **This proves that there is no performance issues to insert multiple partition keys into Azure Storage**.

Appinsights query used

```sql
dependencies
| where name endswith "batch" and (resultCode == 200 or resultCode == 202 or resultCode == 204) and success == 'True' 
| summarize count(), avg(duration) by bin(timestamp, 10m)  
| sort by timestamp asc 
| render timechart 
```

### Querying Tablestorage for Partition Keys

Second part of this experiment was to measure performance of querying and reading the rows based on the partition key. So, I created another function which is again timer triggered for every 30secs. Each execution would randomly query the tablestorage for a partitionkey with a GUID.

This function was also left to run for about a day and the same window was chosen to check for the average dependency duration time chart. Below is the chart plotted for the same.

![Timechart for reading](/assets/images/tspreadgraph.PNG)

Except for the first "aberration" which took about 228ms to complete, rest of the queries executed well within **50ms** latency. Which I feel is optimum considering the SLA.

Query used to chart above graph

```sql
dependencies
| where name startswith "GET" and (resultCode == 200 or resultCode == 202 or resultCode == 204) and success == 'True' 
| summarize count(), avg(duration) by bin(timestamp, 10m)  
| sort by timestamp asc 
| render timechart 
```

## Conclusion
This experiment proves that azure table storage is indeed scalable and can be used to store multiple partition key based data. Microsoft themselves use table storage for a lot of reasons one of them is collecting logs for Azure function. Imagine the amount of data that would be collected if you have a couple of function running every 5 secs for an year or more.

## Points to consider

There are some things to consider while choosing TableStorage as your datastore.

1. **TableStorage has a primary index only on partition key and row key**, hence you would have to select your PK and RK carefully. If you have more than these two fields for querying TableStorage isn't really suited.
2. **There is no upper bound latency**: Tablestorage is fast but Microsoft doesn't provide any latency number, so it could vary as seen in the above timecharts.
3. **Global replication** : TableStorage is part of the Storageaccount, you could have a RA_GRS model with an optional read-only region. But the consistency between the two is eventual and not clearly defined.

The code for the azure functions is available in the [Github](https://github.com/pratap-dotnet/tablestorage-performance)

Happy Coding!!!!