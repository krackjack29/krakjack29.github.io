---
title: "SQL: How to get the primary key of a record after the insert without using Select"
date: 2010/09/13 15:23:00 +530
layout: single
categories: 
   - SQL
tags:
   - sql
   - howto
---

**Question**: How to fetch the automatically incremented primary key after an insert without selecting that particular column?

**Solution**: Use *@@Identity* global variable to find the newly inserted identity/primary key.

In the following example we have a simple table with two columns

1. *Name* : nchar(10)

2. *Id* : int , primary key, auto increment = true

lets define a stored procedure to insert values into the table

```sql
create proc updatetable (@name nchar(10))
As
INSERT INTO [Aesop].[dbo].[Person]
([Name])
VALUES(@name)
return @@IDENTITY
```
And when you execute the above defined procedure, your output variable would contain the newly inserted primary key

```sql
Declare @name nchar(10), @return int,
set @name=‘pratap’;
exec @return = updatetable @name;
print @return
```
More information about the system functions/variables could be found [here](http://msdn.microsoft.com/en-us/library/ms187786.aspx)