---
title: Opening ports on a deployed ServiceFabric cluster
date: 2016/06/28 12:19:00 +530
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
   - howto
---

Service fabric is a Mircoservices platform by Microsoft. It can be run on private cluster running on Windows or Linux , It can also be hosted on Azure.

While deploying service fabric cluster on Azure you can select the endpoints which can be accessed from outside the cluster. You can see more detailed service fabric deployments [here](https://azure.microsoft.com/en-in/documentation/articles/service-fabric-cluster-creation-via-portal/).

But once you have deployed the service cluster and a new service inside the cluster needs an endpoint , then you would have to follow the steps below

1. Open the Azure Portal www.portal.azure.com

2. Navigate to the resource group you deployed your cluster on and select it.

3. You would see a bunch of resources deployed under the group, including cluster, node type, a load balancer, public IP address and a bunch of storage account.

![resource group list](/assets/images/sfstep3.png)

4. Select the Load balancer normally named LB-{clustername}-{nodename}, in the properties tab select “Load Balancing Rules”

![load balancer](/assets/images/sfstep4.png)

5. Click on “+ Add”

![load balancing rule](/assets/images/sfstep5.png)

In the port field mention the port exposed to the public domain, while the backend port should be the port your application on cluster is listening to. Click Ok, it will take about 2-5 minutes to update the nodes and the VMs with new rules and you should be able to access your application.

Happy coding!!!