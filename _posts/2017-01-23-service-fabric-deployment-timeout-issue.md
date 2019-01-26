---
title: "Service fabric deployment–Timeout issue"
date: 2017/01/23 15:39:52 +530
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
Azure service fabric deployment has loads of issues, whether you are trying to deploy using Visual studio or PowerShell one common error you would hit (esp. once your package goes above 100MB) is the Timeout issue.

When you create a new service fabric application, a deployment ps script would be generated within the project folder named “***Deploy-FabricApplication.ps1***“. Since SDK 2.1 MS added a new parameter $CopyPackageTimeoutSec to the script. This variable dictates the timeout interval for the deployment.

But having this parameter alone doesn’t fix the timeout issues as any publish to the service fabric cluster involves 3 main steps

1. Copy-ServiceFabricApplicationPackage – copies the package to the storage of each node in the cluster.
2. Register-ServiceFabricApplicationType – Registers the version of the application to the Service fabric name service
3. New-ServiceFabricApplication or Upgrade-ServiceFabricApplication – Based on existing or new application.

Microsoft even its latest SDK release hasn’t pushed the Timeout variable to the Register-ServiceFabricApplicationType module in the script, which takes almost the same time as Copying the package.

### SOLUTION:
* Open “Publish-NewServiceFabricApplication.ps1” ( located at {InstallFolder}\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK)
* Locate line containing “Register-ServiceFabricApplicationType -ApplicationPathInImageStore $applicationPackagePathInImageStore”
* Add “-TimeOutSec $CopyPackageTimeoutSec” at the end of this line
* Repeat step 2 and 3 for “Publish-UpgradedServiceFabricApplication.ps1”

### NOTE:
The above mentioned solution would be overwritten each time a new SDK version is installed in the system.