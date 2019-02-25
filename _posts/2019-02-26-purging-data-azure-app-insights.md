---
title: "Purging logs from Azure AppInsights"
layout: single
comments: true
date : 2019/02/19
categories:
    - Azure
tags:
    - azure
    - cloud
    - appinsights
    - howto
---

Azure Application Insights is a powerful logging/monitoring service provided by Azure. It can be used to live-stream your application logs, analyze the performance of your application, find out anomalies and other such things. It's easy to set up and configure in your application if you are using  .Net all you have to do is add *APPINSIGHTS_INSTRUMENTATIONKEY* in your config file. If you haven't tried it, please do. Its a must for any application that runs in Azure. 

In this article, we wouldn't be looking at how to populate data in the AppInsights, rather we would be going through on how to delete data from AppInsights.

There could be many reasons why would want to delete or purge data from AppInsights. For e.g. during development stages, you would still be finalizing the log format which might not really make it to production or you might even change the format from an alpha deployment to the actual release of the application etc.

Microsoft consciously has given limited privileges to purge the data. Access is not event provided to resource owner unless explicitly provided. 

> Purge is a highly privileged operation that no app or user in Azure (including even the resource owner) will have permissions to execute without explicitly being granted a role in Azure Resource Manager. This role is Data Purger and should be cautiously delegated due to the potential for data loss.

Once **Data Purger** role is explicitly assigned to the user, he/she can run a REST API (shown below) with to purge the data.

```http
POST 
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Insights/components/{resourceName}/purge?api-version=2015-05-01
```
Where
* subscriptionId - Azure subscription Id
* resourceGroupName - Name of the resource group where the application insights resides
* resourceName - Name of the appinsights

The payload for the above API should be of the following form
```json
{
  "table": "{tableName}",
  "filters": [
    {
      "column": "timestamp",
      "operator": ">",
      "value": "2017-09-01T00:00:00"
    }
  ]
}
```
Where 
* tableName : Name of the table you want data to be purged. 

To list down all the tables that are available in your instance of AppInsights, click on the 'Analytics' icon in azure portal
![App Insights Analytics](/assets/images/purge/pp1.png)
which would open up query editor, which lists down the tables that you might want to delete or purge data from.
![App Insights Schema](/assets/images/purge/pp2.png)

**Important Note**
Microsoft doesn't delete the data immediately, it has an SLA of 30 days. So once you have requested for purging the data you can checkback the status using the below URI.

```http
GET 
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Insights/components/{resourceName}/operations/{purgeId}?api-version=2015-05-01
```
Where
* subscriptionId - Azure subscription Id
* resourceGroupName - Name of the resource group where the application insights resides
* resourceName - Name of the appinsights
* purgeId - Output of the Purge API

Happy Coding!!!
