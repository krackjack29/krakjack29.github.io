---
title: "SQL: How to get youngest and oldest from a person table"
date: 2010/09/08 14:15:00 +530
layout: single
categories: 
   - SQL
tags:
   - sql
   - howto
---

Problem: To fetch the youngest and oldest of the family based on the last name from the “Person” table whose design is as given below in a single query

![Data](/assets/images/image.png)

Solution :


```sql
select ActualTable.FirstName,ActualTable.LastName, 
    DATEDIFF(YY,ActualTable.DateOfBirth,GETDATE()) as Age        
from 
    Person as ActualTable,
    (    select MAX(DateOfBirth) young,MIN(DateOfBirth) old,lastname 
        from Person 
        group by LastName
    ) as AggragatedTable
where 
    ActualTable.DateOfBirth = AggragatedTable.young Or 
    ActualTable.DateOfBirth = AggragatedTable.old and 
    ActualTable.LastName = AggragatedTable.LastName        
order by ActualTable.LastName, Age desc
```

Here i have used an aggregated result from the person table.