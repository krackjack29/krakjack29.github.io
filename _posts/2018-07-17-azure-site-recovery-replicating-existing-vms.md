---
title : "Azure Site Recovery–Replicating existing VMs using ARM Templates"
date: 2018/07/17 
layout: single
comments: true
categories: 
    - Azure
tags:
    - azure
    - cloud
    - vms
    - asr
    - recovery
excerpt: "Automating azure site recovery or ASR to replicate VMs in a secondary region"
---

Azure Site Recovery (ASR) service contributes for the backup and disaster recovery service. It provides features like backup service and disaster recovery service for the IAAS systems. Not only VMs on Azure but VMs in your datacenter running on VMWare and HyperV.

Read below at your own peril, if you are in category of TLDR here is the link for the ARM templates [Github](https://github.com/pratap-dotnet/azure-site-recovery-automation).

Microsoft provides multiple ways of automating the deployments in azure, using CLI, REST Api, Powershell and ARM templates. ASR is a complicated service with lot of sub-resources involved. Unfortunately, those resources wouldn’t be visible when you use resource explorer in the portal as well.

There is very little or no documentation on the ARM templates for enabling replication on site recovery. Even powershell and rest apis are limited to securing VMs which with classic disks, managed disks are not supported through powershell and rest api yet.

This blog post will take you through the process of replicating Azure VMs from one region to another region using ASR.

Overall architecture of a replication would look something like the below picture

![ASR](/assets/images/asrfabrics.png)

To enable replication of VMs in the ASR, you would have to do the following steps.

1. Create a Replication fabric for the source location (You can create only one replication fabric per location)
2. Create a Replication fabric for the target location.
3. Create a retention policy, containing the RTO objectives for your scenario.
4. Create source replication container within source fabric.
5. Create target replication container within target fabric
6. Map source replication container with target replication container.
7. Map Source VNet and Target VNet , so that when recovery happens Vnet configurations is up to date
8. Add existing VMs in the source container as Protectable Item.

If you have read it till here, I am sure you would be lost or confused about all the jargons.

Happy coding!!!

### References
[https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-overview](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-overview)

[https://docs.microsoft.com/en-us/azure/site-recovery/azure-to-azure-powershell](https://docs.microsoft.com/en-us/azure/site-recovery/azure-to-azure-powershell)

[https://docs.microsoft.com/en-us/azure/templates/microsoft.recoveryservices/vaults/replicationfabrics](https://docs.microsoft.com/en-us/azure/templates/microsoft.recoveryservices/vaults/replicationfabrics)