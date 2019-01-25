---
title: Deploying ARM Template and parsing the outputs in a .Net Application
date: 2016/11/04 15:54:00 +530
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

There are scenarios where one might have to provision a Azure resource on the fly and use the connection string or SAS key or credentials of those deployed resources further in your application.

In this blog post we will deploy an Azure IotHub and get the SAS key to do further device management operations.

* A simple ARM template to deploy the Azure IotHub would look like [this](https://github.com/pratapbhaskar/arm-template-deployer/blob/master/ArmTemplateDeployer/iotHubDeploy.json). It contains 4 parameters namely iotHubName, iotHubSku, iotHubTier, iotHubCapacity all the parameters have a default value set except for iotHubName . It emits two ouputs ![](/assets/images/armoutput.png) iotHubHostName , is the name of the resource that we just deployed, while iotHubKeys would emit all the SAS keys generated.
  
* You need service principal and secret to get the access token. ResourceManagerProvider class has a method called GetResourceManagementClient which gets the token using ServicePrincipalId and ServicePrincipalSecret.
  
```csharp
using System;
using Microsoft.Azure.Management.ResourceManager;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using Microsoft.Rest;
 
namespace ArmTemplateDeployer
{
    public class ResourceManagerProvider
    {
        private const string AuthenticationAuthority = "https://login.windows.net/{0}";
        private const string ResourceProviderUri = "https://management.core.windows.net/";
        private const string ApiVersion = "2015-08-15-preview";
 
        private readonly string clientId;
        private readonly string clientSecret;
 
        public ResourceManagerProvider(string clientId, string clientSecret)
        {
            this.clientId = clientId;
            this.clientSecret = clientSecret;
        }
 
        private string GetAccessToken(string tenantId)
        {
            var authenticationContext = new AuthenticationContext(string.Format(AuthenticationAuthority, tenantId));
            var credential = new ClientCredential(clientId: clientId, clientSecret: clientSecret);
            var result = authenticationContext.AcquireTokenAsync(resource: ResourceProviderUri, clientCredential: credential).Result;
             
            if (result == null)
            {
                throw new InvalidOperationException("Failed to obtain the JWT token");
            }
            return result.AccessToken;
        }
 
        public IResourceManagementClient GetResourceManagementClient(string subscriptionId, string tenantId)
        {
            var accessToken = GetAccessToken(tenantId);
            var tokenCredentials = new TokenCredentials(accessToken);
            var client = new ResourceManagementClient(tokenCredentials);
            client.HttpClient.DefaultRequestHeaders.Authorization = new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", accessToken);
            client.SubscriptionId = subscriptionId.ToString();
            return client;
        }
    }
}
```

* Deployer class deploys the arm template, which I have here used as an embedded resource in the assembly but can be an external file as well.  The method DeployAsync deploys the IotHub into a resourcegroup specified.
  
```csharp
public async Task<DeploymentResponse> DeployAsync(string iotHubName, string resourceGroupName, string location)
{
     var parameters = new Dictionary<string, Dictionary<string, object>>
     {
             {"iotHubName", new Dictionary<string, object> { {"value", iotHubName} } }
      };
    var resourceGroupResponse = await resourceManagementClient.ResourceGroups.CreateOrUpdateAsync(resourceGroupName,
                new ResourceGroup { Location = "SouthEast asia" });
             
      var deploymentExtended = await resourceManagementClient.Deployments.CreateOrUpdateAsync(resourceGroupName, iotHubName,
                new Deployment
                {
                    Properties = new DeploymentProperties
                    {
                        Mode = DeploymentMode.Incremental,
                        Template = JObject.Parse(GetEmbeddedResourceAsString("iotHubDeploy")),
                        Parameters = parameters
                    }
                });
            var hasSucceeded = deploymentExtended.Properties.ProvisioningState == "Succeeded";
            return !hasSucceeded ? new DeploymentResponse { IsSuccess = hasSucceeded }
                    : new DeploymentResponse
                    {
                        IsSuccess = hasSucceeded,
                        Outputs = deploymentExtended.Properties.Outputs != null ?
                            JObject.Parse(deploymentExtended.Properties.Outputs.ToString()).ToObject<Dictionary<string, Dictionary<string, object>>>()
                            : new Dictionary<string, Dictionary<string, object>>()
         };
  }
```

The output property would be a json key value pair, which can be serialized into a Dictionary<string, Dictionary<string,object>>

```json
{
    "iotHubHostName":{
        "value" : "whatever is the hubname",
        "type" : "string"
    },
    "iotHubKeys":{
        "value": [
            {
                "KeyName" : "",
                "PrimaryKey" : "",
                "SecondaryKey" :"",
                "Rights":""
            }
        ]
    }
}
```

All the above mentioned source code is available at my [GitHub](https://github.com/pratapbhaskar/arm-template-deployer) page.

— Happy Coding

