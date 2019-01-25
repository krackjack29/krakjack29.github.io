---
title: "LINQ - Generic Sort"
date: 2014/08/14 22:19:00 +530
layout: single
categories: 
   - .Net
tags:
   - linq
   - csharp
---

We faced a scenario where we had a dataset (EF Linq query) which needs to be sorted based on the user selection (user was given the selection option with the table columns display name), we ended up making an extension method which could be used by anyone for dynamically selecting the sorting key.

We used Linq expressions and reflection to create parameter and property selector from the column name. Below is the code

```csharp
public static IQueryable<T> Sort<T>(this IQueryable<T> source, string propertyName, bool isDescending)
{
    if (source == null)
        return null;

    var entityType = typeof(T);
    var property = GetPropertyType(entityType, propertyName);

    if (property == null)
    {
        return source;
    }
    var sortOrder = isDescending ? "OrderByDescending" : "OrderBy";

    var linqParamExpression = Expression.Parameter(entityType, "param");
    var propertyExpression = GetPropertyExpression(propertyName, linqParamExpression);

    var delegateType = typeof (Func<,>).MakeGenericType(typeof (T), propertyExpression.Type);
    var expression = Expression.Lambda(delegateType, propertyExpression, linqParamExpression);

    var orderByExpression = Expression.Call(typeof (Queryable), sortOrder,
        new [] {source.ElementType, propertyExpression.Type}
        , source.Expression, expression);
    return source.Provider.CreateQuery<T>(orderByExpression);
}
```

GetPropertyType gets the property of the class whose name is specified and allows to navigate to the child object property as well. e.g “AssignedToUser.FirstName” would select the child object AssignedToUser’s firstname property for the sorting

```csharp
private static PropertyInfo GetPropertyType(Type entityType, string propertyName)
{
    var splitStringOnDot = propertyName.Split(new[] { '.' }, StringSplitOptions.RemoveEmptyEntries);

    switch (splitStringOnDot.Length)
    {
        case 2:
            {
                var foreignKeyProperty = entityType.GetProperty(splitStringOnDot[0]);
                if (foreignKeyProperty != null)
                {
                    var selectedProperty = foreignKeyProperty.PropertyType.GetProperty(splitStringOnDot[1]);
                    return selectedProperty;
                }
            }
            break;
        case 1:
            return entityType.GetProperty(propertyName);
        default:
            break;
    }
    return null;
}
```

Happy Coding!!!