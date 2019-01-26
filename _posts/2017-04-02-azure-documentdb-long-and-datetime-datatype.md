---
title: "Azure DocumentDb: long and DateTime datatype"
date: 2017/04/02 12:29:00
layout: single
comments: true
categories: 
   - Azure
tags:
   - azure
   - cloud
   - .net
   - csharp
   - cosmosdb
---

One good thing i liked about Service Fabric is the actor systems, which led me a Java port of Akka/Scala called Akka.Net. You must have a look at it, actors are new paradigm for programming, one can create a scalable concurrent distributed system within minutes. This blog isn’t about Akka.Net , they have got great documentation about it [here](http://getakka.net/docs/).

Akka.Net has a persistence layer, to store actor states, which works with most data sources and also comes with extensible framework so that you can create your own. They didn’t have any DocumentDb libraries, since i was using DocumentDb as data source in our project I decided to write one. You can find the source code at [https://github.com/pratapbhaskar/Akka.Persistence.DocumentDb](https://github.com/pratapbhaskar/Akka.Persistence.DocumentDb).

While creating this project I came across a unique problem in DocumentDb. To maintain order in the snapshot store of Akka actors we need to persist the time of snapshot creation with precision, If stored as Datetime wouldn’t have the capability of queries other than equal to. More information [here](https://azure.microsoft.com/en-in/blog/working-with-dates-in-azure-documentdb-4/).

Hence **DateTime** is stored as **Ticks** (what is a Tick?? explained [here](http://stackoverflow.com/questions/9644608/datetime-ticks-in-net-its-usage-and-why-it-is-used)).  Ticks would normally generate a long number e.g. 636267296223504225. DocumentDb works with Json documents and JavaScript can handle up to 2^53 only. Hence we serialize Ticks (long) it loses precision.

```csharp
public class SnapshotEntry
{
    [JsonProperty("id")]
    public string Id { get; set; }
    public string PersistenceId { get; set; }
    public long SequenceNr { get; set; }
    public long Timestamp { get; set; }
    public object Snapshot { get; set; }
    public DateTime DateTime { get; set; }
}
```

Timestamp is fed with value of **636267314215174643** but when serialized to documentDb object following is what is persisted.

```json
{
    "id": "12",
    "PersistenceId": "1",
    "SequenceNr": 0,
    "Timestamp": 636267319552566500,
    "Snapshot": null,
    "DateTime": "2017-04-02T12:12:35.2576572+05:30",
    "_rid": "sr0XAPA8OQADAAAAAAAAAA==",
    "_self": "dbs/sr0XAA==/colls/sr0XAPA8OQA=/docs/sr0XAPA8OQADAAAAAAAAAA==/",
    "_etag": "\"00000a00-0000-0000-0000-58e09d5b0000\"",
    "_attachments": "attachments/",
    "_ts": 1491115355
}
```

Notice the list precision ??. Now there are two ways to solve this problem

**Simple approach: Store the long values as a String object.** But it would reduce the capability of using greather than >= and other number filters on the field. You might have to load the objects in memory and then filter (bad for performance).

**Complex approach: Divide the Datetime into two parts.** First part the date in yyyymmdd format and second one in Ticks starting from day start.

```csharp
public class DateTimeJsonObject
{
    public int Date { get; set; }
    public long Ticks { get; set; }
    public string TotalTicks { get; set; }

    /// <summary>
    /// Initializes a new instance of the <see cref="DateTimeJsonObject"/> class.
    /// </summary>
    /// <param name="dateTime">The date time.</param>
    public DateTimeJsonObject(DateTime dateTime)
    {
        var year = dateTime.Year.ToString().PadLeft(4, '0');
        var month = dateTime.Month.ToString().PadLeft(2, '0');
        var date = dateTime.Day.ToString().PadLeft(2, '0');

        Date = Convert.ToInt32($"{year}{month}{date}");
        Ticks = dateTime.Subtract(dateTime.Date).Ticks;
        //Since we are calculating Ticks only since start of the date it would be less than 2^53 which is json stored
        TotalTicks = dateTime.Ticks.ToString();
    }
    /// <summary>
    /// Initializes a new instance of the <see cref="DateTimeJsonObject"/> class.
    /// </summary>
    public DateTimeJsonObject()
    {

    }

    /// <summary>
    /// To the date time.
    /// </summary>
    /// <returns></returns>
    public DateTime ToDateTime()
    {
        return new DateTime(long.Parse(TotalTicks));
    }
}
```

Now we could make queries to find greater than like following

```csharp
var dateTimeAsJson = new DateTimeJsonObject(criteria.MaxTimeStamp);
               query = query.Where(a => a.Timestamp.Date < dateTimeAsJson.Date || 
                   a.Timestamp.Ticks <= dateTimeAsJson.Ticks);
```