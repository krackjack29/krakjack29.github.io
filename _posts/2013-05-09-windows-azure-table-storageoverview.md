---
title: Windows Azure Table Storage
date: 2013/05/09 19:32:00 +530
layout: single
comments: true
categories: 
   - Azure
tags:
   - azure
   - cloud
---

Windows Azure Storage account provides access to Blob, Queue and Table Storage services. By default we get up to 100 TB storage (all 3 types of storage including).Read more about it [here](http://www.windowsazure.com/en-us/manage/services/storage/what-is-a-storage-account/).

Table storage is a No-Sql storage which stores data (entities, .Net objects) as rows in a table. The entities are indexed based on the partition key and row key. Combination of these two properties would form a unique identifier for the entity.

The image below gives a conceptual storage model of the windows azure storage account and table storage.

![Tablestorage](/assets/images/tablestorage.png)

## Advantages of Table Storage

* Its cheap, 100 GB storage account costs around $10 per month , while SQL Database on cloud for a storage size of 10GB costs $45 per month.  You can check for yourself here.
* Table storage provides querying capabilities. One can easily access the entities using Partition keys and Row keys. (Querying using properties of the entity other than PK and RK is also supported, but its a performance hazard).
* Its POCO, the data that needs to be stored is Pure Old classes and objects.
* The azure storage library also supports OData protocol to access the entities which means RESTful APIs to access data.

## Disadvantages of Table Storage
* The entities should be less than 1MB of size, so large datasets cannot be stored.
* Its No-Sql storage hence you cannot create relationships between two tables. Of course no joins or any SQL kind of stuff.

By the above mentioned advantages and disadvantages one could gauge when to use Table Storage and not.

Next blog is about programming windows azure table storage.