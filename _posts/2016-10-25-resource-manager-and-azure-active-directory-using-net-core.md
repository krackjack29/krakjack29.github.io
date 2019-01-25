---
title: Resource Manager And Azure Active Directory using .Net Core
date: 2016/10/25 23:46:00 +530
layout: single
categories: 
   - Azure
tags:
   - azure
   - cloud
   - .net
   - csharp
   - azureactivedirectory
   - azure resource manager
---


I just published a github [repository](https://github.com/pratapbhaskar/azure-resource-manager-active-directory) containing .Net core web application to authenticate Azure Resource Manager APIs and gain access to resources in other subscriptions.

This solution is built in the same lines of solution proposed by Microsoft, [https://azure.microsoft.com/en-us/documentation/articles/resource-manager-api-authentication/](https://azure.microsoft.com/en-us/documentation/articles/resource-manager-api-authentication/) , which was built on ASP.Net MVC. 

There were some interesting learning while building it

* Dependency Injection in .Net Core
* OpenIdConnect middleware in .Net Core
* Custom Middleware

– Happy Coding!!!
