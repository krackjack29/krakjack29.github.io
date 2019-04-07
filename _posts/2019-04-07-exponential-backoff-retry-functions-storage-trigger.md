---
title: "Retry logic for Azure Functions with Queue Trigger"
layout: single
comments: true
date: 2019/04/07
categories:
    - Azure
tags:
    - azure
    - cloud
    - functions
    - queue
    - patterns
---
Azure functions with Storage Queue trigger has a built in retry logic based on the dequeue count of the message in the queue. You can configure the frequency of the retry in the bindings either through code or host.json.

```json
{
    "version": "2.0",
    "extensions": {
        "queues": {
            "maxPollingInterval": "00:00:02",
            "visibilityTimeout" : "00:00:30",
            "batchSize": 16,
            "maxDequeueCount": 5,
            "newBatchThreshold": 8
        }
    }
}
```

*visibilityTimeout* property dictates the interval between each retry, *maxDequeueCount* dictates the number of retries on the function. If there is an exception each time a function executes the message is automatically put back in the queue by the runtime and retried after 30s. This would continue for about 5 times.

You can modify the *host.json* only during deployment, a scenario where you would have to retry in a manner where the function has to retry in a increasing interval.

A Typical scenario would be a dependency of a third party application which could have a varied latency or connectivity.

Solution to the custom retry policy would be to add a *DequeueCount* property to the message and clone the message into a new message each time error happens with an increasing visibility timeout and enqueue the message back to the queue.

```csharp
if(message.DequeueCount > 5){
    var nextVisibleTime = TimeSpan.FromSeconds(message.DequeueCount * 30);
    message.DequeueCount++;
    await _queue.AddMessageAsync(new CloudQueueMessage
(JsonConvert.SerializeObject(message.Clone())),
null, nextVisibleTime, new QueueRequestOptions(), new OperationContext());
}
```

Above code would enqueue the message with a 30s delay for the first time, then by a min and so on, till dequeueCount is more than the configured limit.

Happy Coding!!!