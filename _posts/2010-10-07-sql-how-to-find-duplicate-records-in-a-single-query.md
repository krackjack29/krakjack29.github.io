---
title: "SQL: How to find duplicate records in a single query"
date: 2010/10/07 16:26:00 +530
layout: single
categories: 
   - sql
tags:
   - sql
   - howto
---

**Problem**: Need a dataset containing all the duplicate records based on a particular criteria selected by the user.

**Solution**: Following query would give the dataset (SQL) containing all the duplicate records for contact table

```sql
select (First_Name + Last_Name) as Criteria, *
from Contact TGT 
where (First_Name + Last_Name) 
in 
(
    select (First_Name + Last_Name) from Contact 
    Group by (First_Name + Last_Name) 
    having COUNT(First_NAme + Last_Name ) > 1  
)
```

In the above query the user has selected First_Name and Last_Name as the criteria, if user selects address, create a new criteria.

Result would be something like this

![sql result](/assets/images/sqlresult.png)