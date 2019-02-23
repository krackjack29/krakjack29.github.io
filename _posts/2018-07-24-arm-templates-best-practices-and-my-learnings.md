---
title: "Arm templates–Best practices and my learnings"
layout: single
comments: true
date: 2018/07/24 
categories: 
    - Azure
tags:
    - azure
    - cloud
    - arm templates
---

I have been lately working on ***Infrastructure as Code*** project to deploy reference architectures on Microsoft Azure, which provided me with an opportunity to do a deep dive into the world or Azure Resource Manager (ARM) templates. This blog is to summarize the experience and learnings about ARM templates.

## Linked Templates
To improve the reusability of the templates, always use linked templates approach. Break down the resources you need to deploy into granular pieces as individual templates and invoke them using a root template.

Point to remember: The nested templates must be visible to the Azure resource manager during deployment. One can deploy all the templates to a private blob storage and generate a SAS key during the deployment.

```json
{
    "type": "Microsoft.Resources/deployments",
    "name": "keyVaultDeployment",
    "apiVersion": "2017-05-10",
    "properties": {
        "mode": "Incremental",
        "templateLink": {
            "uri": "[uri(parameters('_artifactsLocation'),concat('nestedtemplates/AzureDeploy-KeyVault.json', parameters('_artifactsSASToken')))]",
            "contentVersion": "1.0.0.0"
        },
        "parameters": {
        }
    }
}
```
In the above snippet “_artifactsLocation” would be the path for the template, while “_artifactsSASToken” would be the SAS key to access that path during execution.

## Known Configurations
Azure provides offers a huge number of options for each resource e.g. Redis cache has more than 10 different offerings. Instead of possibly listing all the options in the parameters, it’s a good practice to categorize the sizes/skus into T-shirt size such as Small, Medium or Large.

Recommendation: Create templates which will give out these sizes as outputs which can be used by other nested template deployments. Assuming you have 3 t-shirt sizes small, medium and large, you would end up having “KnownConfig-Small.json”, “KnownConfig-Large.json” and “KnownConfig-Medium.json”. The root template would take deployment size as one of the parameters and executes the appropriate known config template by appending the parameter.

```json
{
    "type": "Microsoft.Resources/deployments",
    "name": "knownConfigurations",
    "apiVersion": "2017-05-10",
    "properties": {
        "mode": "Incremental",
        "templateLink": {
            "uri": "[uri(parameters('_artifactsLocation'),concat('nested/KnownConfigurations-',parameters('deploymentSize'),'.json', parameters('_artifactsSASToken')))]",
            "contentVersion": "1.0.0.0"
        },
        "parameters": {}
    }
}
```
Known configuration file would look something like below, you could all the resources type as output in the same file.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "resources": [],
    "outputs": {
       "cacheSettings": {
            "type": "object",
            "value": {
                "sku": "Premium",
                "skuFamily": "P",
                "skuCapacity": 3
            }
        },
        "sqlSettings": {
            "type": "object",
            "value": {
                "databaseCollation": "SQL_Latin1_General_CP1_CI_AS",
                "performanceLevel": "Basic"
            }
        }
    }
}
```

## DependsOn property
Always use the “dependsOn” property to create a hierarchy of deployments. By default, ARM deployments are parallelized for optimized performance.  DependsOn property would actually block the current deployment till the deployment that depended upon would complete.

The use of reference keyword would result in the same effect, but its good practice to clearly specify the parent resources in dependsOn property.

## Minor Stuff
* Reference function actually verifies whether the resource exists or not.
* Reference function is available only at runtime and cannot be accessed within variables section
* If you have multiple extensions being deployed on a VMScaleset, be careful they run in parallel and could cause undesired effects.
* One could specify the values of parameters as a value or a reference, this is really useful when providing passwords, instead of directly passing the value you could pass in the keyvault id like this

```json
"sqlpassword": {
    "reference": {
        "keyVault": {
            "id": "/subscriptions/{subId}/resourceGroups/{rgId}/providers/Microsoft.KeyVault/vaults/{vaultName}"
        },
        "secretName": "{secretname}"
    }
}
```
### Useful resources
[https://docs.microsoft.com/en-us/azure/templates/](https://docs.microsoft.com/en-us/azure/templates/)

[ARM Template reference](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-template-functions) 

[Template function reference](https://docs.microsoft.com/en-us/azure/azure-resource-manager/)

Happy Coding!!!