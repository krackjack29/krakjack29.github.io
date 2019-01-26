---
title: Partitioning and retrieving all the actors in service fabric
date: 2016/07/09 11:22:00 +530
layout: single
comments: true
categories: 
   - Azure
tags:
   - azure
   - cloud
   - .net
   - csharp
   - servicefabric
---

A Reliable actor service is partitioned across the service fabric cluster. Partitioning in the context of service fabric is process of determining the particular service partition is responsible for complete state of service.

By default service fabric uses “ranged partitioning” for the stateful/actor service. In ranged partitioning the actors are divided into n number of buckets (n being the partition count of the service) with each bucket capable of holding x number of keys/partition based on low key and high key.

e.g. Lets assume you have an actor service with partition count of 5, high key = 100 & low key =  –100. (you can find the default values in the ApplicationManifest.xml), the keys or actorIds would fall in below bucket range.

![Partition buckets](/assets/images/sfpartition.png)

When an Actor with ActorId 15 is created Partition 3 maintains the state for that instance. Now there are scenarios when you would want to retrieve all the actors of a type in Service Fabric cluster, below is the code to achieve the same.

```csharp
public async Task GetActiveActors(CancellationToken cancellationToken = default(CancellationToken))
{
    var highKey = 100; 
    var lowKey = -100;
    var partitionCount = 5;
    var randomNumer = new Random();
    var partitionRange = highKey/partitionCount - lowKey/ partitionCount; //40
 
    List<ActorInformation> actorInformationList = new List<ActorInformation>();
    for (int i = 0; i < partitionCount; i++)
    {
        //this would generate keys as -100, -60, -20, 20, 60
        var randomPartitionKey = i * partitionRange + lowKey;
        var actorProxy = ActorServiceProxy.Create(new Uri("{your service fabric Uri}"), randomPartitionKey);
        ContinuationToken continuationToken = null;
        do
        {
            var page = await actorProxy.GetActorsAsync(continuationToken, cancellationToken);
            actorInformationList.AddRange(page.Items);
            continuationToken = page.ContinuationToken;
        } while (continuationToken != null);
 
    }
}
```

The variables in the above code can be picked up easily from the ApplicationManifest.xml.

Happy Coding!!

### References
[https://azure.microsoft.com/en-in/documentation/articles/service-fabric-reliable-actors-platform/](https://azure.microsoft.com/en-in/documentation/articles/service-fabric-reliable-actors-platform/)
[https://azure.microsoft.com/en-in/documentation/articles/service-fabric-concepts-partitioning/](https://azure.microsoft.com/en-in/documentation/articles/service-fabric-concepts-partitioning/)