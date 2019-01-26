---
title: TableEntity and TableStorage
date: 2013/07/15 20:10:00 +530
layout: single
comments: true
categories: 
   - Azure
tags:
   - azure
   - cloud
   - .net
   - csharp
---

In the previous blog we just went through with the advantages of using TableStorage, in this one we would consider the most important Class to be used to save the entities.

There is a very good walkthrough by Microsoft about how to use the table storage programmatically [here](http://www.windowsazure.com/en-us/develop/net/how-to-guides/table-services/).  I will share my experiences with table storage.

## TableServiceEntity and TableEntity

Either one of these classes (Microsoft.WindowsAzure.Storage.Table.TableEntity and Microsoft.WindowsAzure.Storage.Table.DataServices.TableServiceEntity) has to be inherited by the entity that we choose to save in the table storage.

These two classes are totally different and have no relationship with each other. TableEntity is the latest class published as part of Azure Storage SDK 2.0, while TableServiceEntity was available earlier has well.

The main difference i noticed with these two classes is that TableEntity gives more customizability and control to reading and writing the entities to the storage. And TableServiceEntity uses the WCF data services protocol to do any operations and most of the APIs wouldn’t work, like AddLinkedObject etc.

Both classes have 3 main properties

* PartitionKey
* RowKey
* ETag

## Things to consider while using Table Storage
* The Partition Key and RowKey : These properties have to be populated and are primary identifiers for your entity. These are of type “System.string”. They have to be not more than 1 KB of size that is around 256 characters if UTF 16 encoding is selected. Best article to read about selecting PartitionKey and RowKey [http://msdn.microsoft.com/en-us/library/windowsazure/dd179338.aspx](http://msdn.microsoft.com/en-us/library/windowsazure/dd179338.aspx) .
* Properties of Entity : Only certain types of properties are allowed by default in the Entity and are listed below in the table. Courtesy same link

| WCF DataServics Type       | CLR Type           | Details  |
|:------------- |:-------------|:-----|
|Edm.Binary|byte[]|An array of bytes up to 64 KB in size.|
|Edm.Boolean|bool|A Boolean value.|
|Edm.DateTime|DateTime|A 64-bit value expressed as Coordinated Universal Time (UTC). The supported DateTime range begins from 12:00 midnight, January 1, 1601 A.D. (C.E.), UTC. The range ends at December 31, 9999.|
|Edm.Double|double|A 64-bit floating point value.|
|Edm.Guid|Guid|A 128-bit globally unique identifier.|
|Edm.Int32|Int32 or int|A 32-bit integer.|
|Edm.Int64|Int64 or long|A 64-bit integer.|
|Edm.String|String|A UTF-16-encoded value. String values may be up to 64 KB in size.|

if one needs more types to be supported he would have to override the WriteEntity and ReadEntity methods from the TableEntity class.

Best practice would be to create a base class of your own and inherit the TableEntity supporting new types for the properties and also checking for other conditions like the size of the property. Below is my version of that base table entity class.

```csharp
using System;
using System.Collections.Generic;
using System.Reflection;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Table;
using Microsoft.WindowsAzure.Storage.Table.DataServices;

namespace P2Reader.Entity.Framework
{
    /// <summary>
    /// Abstract table service entity to abstract the Entities
    /// </summary>
    public class BaseTableEntity : TableEntity
    {   
        #region Constructor
        /// <summary>
        /// Initializes a new instance of the <see cref="BaseTableEntity"/> class.
        /// </summary>
        public BaseTableEntity() : base() { }

        /// <summary>
        /// Initializes a new instance of the <see cref="BaseTableEntity"/> class.
        /// </summary>
        /// <param name="partitionKey">The partition key of the <see cref="T:Microsoft.WindowsAzure.Storage.Table.TableEntity" /> to be initialized.</param>
        /// <param name="rowKey">The row key of the <see cref="T:Microsoft.WindowsAzure.Storage.Table.TableEntity" /> to be initialized.</param>
        public BaseTableEntity(string partitionKey, string rowKey) : base(partitionKey, rowKey) { }
        #endregion

        #region Overriden members - To Support more types
        /// <summary>
        /// Serializes the <see cref="T:System.Collections.Generic.Dictionary`2" /> of property names mapped to <see cref="T:Microsoft.WindowsAzure.Storage.Table.EntityProperty" /> data values from this <see cref="T:Microsoft.WindowsAzure.Storage.Table.TableEntity" /> instance.
        /// </summary>
        /// <param name="operationContext">An <see cref="T:Microsoft.WindowsAzure.Storage.OperationContext" /> object used to track the execution of the operation.</param>
        /// <returns>
        /// A map of property names to <see cref="T:Microsoft.WindowsAzure.Storage.Table.EntityProperty" /> data typed values created by serializing this table entity instance.
        /// </returns>
        public override IDictionary<string, EntityProperty> WriteEntity(OperationContext operationContext)
        {
            var dictionaryToReturn = base.WriteEntity(operationContext);
            var propertiesWhichAreOfSpecialType = GetPropertiesOfSpecialType();

            foreach (var property in propertiesWhichAreOfSpecialType)
            {
                if (!dictionaryToReturn.ContainsKey(property.Name))
                {
                    dictionaryToReturn.Add(property.Name, new EntityProperty(property.GetValue(this).ToString()));
                }
            }
            FilterPropertiesForSpecialScenarios(dictionaryToReturn);
            return dictionaryToReturn;
        }

        /// <summary>
        /// Filters the properties for special scenarios.
        /// </summary>
        /// <param name="dictionaryToReturn">The dictionary to return.</param>
        private void FilterPropertiesForSpecialScenarios(IDictionary<string, EntityProperty> dictionaryToReturn)
        {
            foreach (var item in dictionaryToReturn)
            {
                //Truncate the size of the string to 64KB
                if (!string.IsNullOrEmpty(item.Value.StringValue) && item.Value.StringValue.Length > 32000)
                {
                    item.Value.StringValue = item.Value.StringValue.Substring(0, 32000);
                }
            }
        }

        /// <summary>
        /// De-serializes this <see cref="T:Microsoft.WindowsAzure.Storage.Table.TableEntity" /> instance using the specified <see cref="T:System.Collections.Generic.Dictionary`2" /> of property names to <see cref="T:Microsoft.WindowsAzure.Storage.Table.EntityProperty" /> data typed values.
         /// </summary>
         /// <param name="properties">The map of string property names to <see cref="T:Microsoft.WindowsAzure.Storage.Table.EntityProperty" /> data values to de-serialize and store in this table entity instance.</param>
         /// <param name="operationContext">An <see cref="T:Microsoft.WindowsAzure.Storage.OperationContext" /> object used to track the execution of the operation.</param>
         public override void ReadEntity(IDictionary<string, EntityProperty> properties, OperationContext operationContext)
         {
             base.ReadEntity(properties, operationContext);
             var propertiesWhichAreOfSpecialType = GetPropertiesOfSpecialType();
 
             foreach (var property in propertiesWhichAreOfSpecialType)
             {
                 EntityProperty value;
                 properties.TryGetValue(property.Name, out value);
                 if (value == null)
                     continue;
 
                 if (property.PropertyType == typeof(Uri))
                     property.SetValue(this,new Uri(value.StringValue));
             }
         }
 
         /// <summary>
         /// Gets the type of the properties of special.
         /// </summary>
         /// <returns></returns>
         private List<PropertyInfo> GetPropertiesOfSpecialType()
         {
             List<PropertyInfo> properties = new List<PropertyInfo>();
             foreach (var property in this.GetType().GetProperties())
             {
                 if (property.Name == "PartitionKey" || property.Name == "RowKey" || property.Name == "Timestamp" || property.Name == "ETag" || property.GetSetMethod() == null || !property.GetSetMethod().IsPublic || property.GetGetMethod() == null || !property.GetGetMethod().IsPublic)
                 {
                     continue;
                 }
                 if (Helper.AllowedTypes.Contains(property.PropertyType))
                 {
                     continue;
                 }
                 properties.Add(property);
             }
             return properties;
         }
         #endregion
     }
 
     internal static class Helper
     {
         public static List<Type> AllowedTypes;
 
         static Helper()
         {
             if (AllowedTypes == null || AllowedTypes.Count == 0)
             {
                 AllowedTypes = new List<Type>();
                 AllowedTypes.AddRange(new Type[]{
                     typeof(string),
                     typeof(byte[]),
                     typeof(Int32),typeof(Int32?),
                     typeof(Int64),typeof(Int64?),
                     typeof(DateTime),typeof(DateTime?),
                     typeof(Guid),typeof(Guid?),
                     typeof(bool),typeof(bool?),
                     typeof(double),typeof(double?)
                 });
             }
         }
     }
 }
```
In my next blog i would explain the repository pattern and how to leverage .Net generics to create table store and perform CRUD operations.