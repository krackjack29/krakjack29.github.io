---
title: ARM Templates to provision Azure Resources
date: 2016/11/04 13:02:00 +530
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

Azure Resource Manager (ARM) templates are quite handy when it comes to automating the provisioning of azure resources. Almost 99% of the azure resources can now be provisioned using these templates.

Microsoft also has quick start templates for all the resources in [GitHub](https://github.com/Azure/azure-quickstart-templates). If you notice the quick start templates they normally have two files in each example

1. azuredeploy.json – contains information of resources that you are going to provision, the json object contains 4 objects internally.
   * parameters : this section allows you to define the inputs for the deployments such as name of the resources etc.
   * variables : contains calculated variables to be used within the scope of the deployment json.
   * resources : resources that are to be deployed, you can set the resource order of deployment using “dependsOn” property.
   * outputs: define the output after the deployment is successful, could be a connection string to a SQL database you deployed, SAS key to the eventhub etc.
2. azuredeploy.parameters.json – contains the parameter values defined in azuredeploy.json.
   
You can deploy the ARM templates using PowerShell, or command line (Azure CLI commands need to be installed for this). To execute above options you would need the azuredeploy.json file to have a open uri endpoint and provide that endpoint as the input to the commands.

You can also deploy the templates using REST APIs and a .Net client. Here is the sample application in [MSDN](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-windows-csharp-template/).

— Happy Coding