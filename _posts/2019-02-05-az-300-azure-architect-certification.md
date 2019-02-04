---
title: 'AZ-300 Azure Architect Technologies Certification'
layout: single
comments: true
date : 2019/02/04
categories:
    - Azure
tags:
    - azure
    - cloud
    - certifications
toc: true
---

I took [AZ-300](https://www.microsoft.com/en-us/learning/exam-az-300.aspx) exam today and cleared it :blush: , this was my first-ever official certification ever. Not that I didn't value them earlier, was never motivated to take one. Even though I have around 5 years of solid experience in developing/architecting solutions on Azure, I hadn't read or worked on many of the services that Azure has. Studying for the above said certification definitely helped in brushing and also exploring newer services that Azure has to offer. 

Exam format was very simple and most of the questions were case study based, hence your experience on the subject do matter. I wanted to share some of the notes that I had made while preparing for the exam topicwise.

## Deploy and Configure Infrastructure (25-30%)

### Analyze resource utilization and consumption

#### Configure diagnostic setting on resources
* [https://azure.microsoft.com/en-gb/blog/azure-monitor-multiple-diagnostic-settings/](https://azure.microsoft.com/en-gb/blog/azure-monitor-multiple-diagnostic-settings/)
* [https://docs.microsoft.com/en-us/azure/azure-monitor/platform/diagnostic-logs-overview](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/diagnostic-logs-overview)
* [https://docs.microsoft.com/en-us/azure/azure-monitor/platform/agents-overview](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/agents-overview)

#### Create baseline for resources - Metrics alerts
* [https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-dynamic-thresholds](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-dynamic-thresholds)
* [https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-metric-overview](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-metric-overview)

#### Create and raise alerts; analyse alerts across subscription
* [https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-metric](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-metric)

#### Monitor for unused resources
* [https://docs.microsoft.com/en-us/azure/advisor/advisor-overview](https://docs.microsoft.com/en-us/azure/advisor/advisor-overview)
* Advisor benefits are : 
  * Get proactive, actionable and personalized best practices
  * Improve performance, security and high availability
  * Proposed actions inline
* Categories
  * [High Availability](https://docs.microsoft.com/en-us/azure/advisor/advisor-high-availability-recommendations)
  * [Security - Security advisor recommendations](https://docs.microsoft.com/en-us/azure/security-center/security-center-recommendations)
  * [Performance](https://docs.microsoft.com/en-us/azure/advisor/advisor-performance-recommendations)
    * Db performance with SQL db advisor
    * App Service performance and reliabilty
    * Use managed disks
    * Premium storage
* [Cost - Cost management advisor (applicable only for enterprise agreement)](https://docs.microsoft.com/en-us/azure/advisor/advisor-cost-recommendations)
  * Virtual machine resizing : <5% CPU for 14 day
  * Express route circuts
  * Idle virtual network gateways
  * Reserved instances vs pay as you go

#### Monitor spend; report on spend
[https://docs.microsoft.com/en-us/azure/billing/billing-getting-started](https://docs.microsoft.com/en-us/azure/billing/billing-getting-started)
You have to be account admin to see billing, ways to monitor your costs
1. Add tags to your resources to group your billing data
2. Regularly check the portal for cost breakdown and burn rate
3. Consider enabling cost cutting features like auto shutdown for VMs
4. Turn on and check on azure advisor recommendations

#### Utilize Log Search query functions
[https://docs.microsoft.com/en-us/azure/azure-monitor/log-query/log-query-overview](https://docs.microsoft.com/en-us/azure/azure-monitor/log-query/log-query-overview)
Log queries are used
* Portals
* Alert rules
* Dashboards
* Views
* Export
* Powershell
* Log Analytics API
* Application insights portal

### Create and configure storage accounts

#### Create and configure Storage account
[https://docs.microsoft.com/en-us/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal](https://docs.microsoft.com/en-us/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal)

#### Configure network access to storage account
[https://azure.microsoft.com/en-gb/blog/announcing-virtual-network-integration-for-azure-storage-and-azure-sql/](https://azure.microsoft.com/en-gb/blog/announcing-virtual-network-integration-for-azure-storage-and-azure-sql/)

[https://docs.microsoft.com/en-us/azure/storage/common/storage-network-security](https://docs.microsoft.com/en-us/azure/storage/common/storage-network-security)

* Firewall rules and Vnet endpoints
* SAS token generated for IP address do not grant access beyond the configured Vnet rules
* Virtual machine disk traffic is not affected by network rules but REST access to blobs are
* Vnet service endpoint for azure storage
* Only primary and paired region Vnets are allowed. i.e. A storage account created in central india can have Vnet created in central india and south india, across subscriptions under same tenant id

#### Monitor activity log by using Log Analytics

Storage account comprising of Diagnostic logs are added by default

#### Generate shared access signature
[https://docs.microsoft.com/en-us/azure/storage/common/storage-dotnet-shared-access-signature-part-1](https://docs.microsoft.com/en-us/azure/storage/common/storage-dotnet-shared-access-signature-part-1)

[Account SAS](https://docs.microsoft.com/en-us/rest/api/storageservices/constructing-an-account-sas)

[Service SAS](https://docs.microsoft.com/en-us/rest/api/storageservices/constructing-a-service-sas)

#### Storage Replication
[https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy](https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy)
* Locally-redundant storage (LRS) - 1 instance in datacenter (11 9's)
* Zone-redundant storage (ZRS) - 3 instances in same datacenter but different zones (12 9's)
* Geo-redundant storage (GRS) - 2 instances in different regions (16 9s)
* Read-access GRS - 2 instance in different region with read access on secondary regions (16 9's), In GRS RPO is less than 15 mins

#### Install and use Azure Storage Explorer
[https://azure.microsoft.com/en-gb/features/storage-explorer/](https://azure.microsoft.com/en-gb/features/storage-explorer/)

#### Manage access keys
[https://docs.microsoft.com/en-us/azure/key-vault/key-vault-ovw-storage-keys](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-ovw-storage-keys) 

[https://docs.microsoft.com/en-us/azure/key-vault/key-vault-ovw-storage-keys](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-ovw-storage-keys)

### Create and configure a VM for windows and linux

#### VMs Important
[https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-manage-vm](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-manage-vm)

	• Disks
		○ OS Disk - Created by default, can go up to 2 TB, should not be used for applications and data
	• Temporary Disk
		○ Created by default, size based on VM size, not for data used by application to process faster. SSD
	• Data Disks
		○ Not created by default, should be used to install applications and store data. For each VM's vCPU 4 data disks can be attached

Standard (HDD) disk and Premium (SSD) disks


Deploy and configure scale sets
https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-create-vmss

Configure High availability
https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-availability-sets
https://docs.microsoft.com/en-us/azure/virtual-machines/linux/manage-availability

configure monitoring, networking, storage, and virtual machine size
https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-monitoring
	• Boot diagnostics are not enabled by default must be configured while creating the VM
	• Host Metrics are collected by default and can be viewed from portal
	• Diagnostic extension needs to be enabled ("Enable guest level monitoring" ) to collect VM metrics and can be viewed from portal
	• Updates to VM can be managed using OMS solution and Hybrid worker concept

https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-virtual-network
https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-create-vmss