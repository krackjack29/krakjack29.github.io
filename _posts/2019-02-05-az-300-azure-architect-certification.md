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
---

I took [AZ-300](https://www.microsoft.com/en-us/learning/exam-az-300.aspx) exam today and cleared it :blush: , this was my first-ever official certification ever. Not that I didn't value them earlier, was never motivated to take one. Even though I have around 5 years of solid experience in developing/architecting solutions on Azure, I hadn't read or worked on many of the services that Azure has. Studying for the above said certification definitely helped in brushing and also exploring newer services that Azure has to offer.

Exam format was very simple and most of the questions were case study based, hence your experience on the subject do matter. I wanted to share some of the notes that I had made while preparing for the exam topicwise.

A big thanks to Gregor Suttie's [curated](https://gregorsuttie.com/2018/10/02/azure-architect-design-az-300-exam/) list of links to study, I have added a few myself along with some highlights.

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

* Disks
  * OS Disk - Created by default, can go up to 2 TB, should not be used for applications and data
* Temporary Disk
  * Created by default, size based on VM size, not for data used by application to process faster. SSD
* Data Disks
  * Not created by default, should be used to install applications and store data. For each VM's vCPU 4 data disks can be attached
* Standard (HDD) disk and Premium (SSD) disks

#### Deploy and configure scale sets

[https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-create-vmss](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-create-vmss)

#### Configure High availability

[https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-availability-sets](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-availability-sets)

[https://docs.microsoft.com/en-us/azure/virtual-machines/linux/manage-availability](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/manage-availability)

#### Configure monitoring, networking, storage, and virtual machine size

[https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-monitoring](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-monitoring)

* Boot diagnostics are not enabled by default must be configured while creating the VM
* Host Metrics are collected by default and can be viewed from portal
* Diagnostic extension needs to be enabled ("Enable guest level monitoring" ) to collect VM metrics and can be viewed from portal
* Updates to VM can be managed using OMS solution and Hybrid worker concept

### Automate deployment of Virtual Machines (VMs)

#### Modify Azure Resource Manager (ARM) template

[https://docs.microsoft.com/en-us/azure/devops/pipelines/apps/cd/azure/build-azure-vm-template?view=azdevops&viewFallbackFrom=vsts](https://docs.microsoft.com/en-us/azure/devops/pipelines/apps/cd/azure/build-azure-vm-template?view=azdevops&viewFallbackFrom=vsts)

[https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates)

[https://docs.microsoft.com/en-us/azure/templates/microsoft.compute/allversions](https://docs.microsoft.com/en-us/azure/templates/microsoft.compute/allversions)

#### Configure location of new VMs

[https://docs.microsoft.com/en-us/azure/virtual-machines/windows/tutorial-manage-vm](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/tutorial-manage-vm)

[https://docs.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-portal](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-portal)

#### Configure VHD template

[https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sa-create-vm-specialized](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sa-create-vm-specialized)

[https://docs.microsoft.com/en-us/azure/virtual-machines/windows/create-vm-specialized](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/create-vm-specialized)

[https://docs.microsoft.com/en-us/azure/virtual-machines/windows/prepare-for-upload-vhd-image](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/prepare-for-upload-vhd-image)

#### Deploy from template, Save a deployment as a template

[https://docs.microsoft.com/en-us/azure/virtual-machines/windows/ps-template](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/ps-template)

#### Deploy windows and linux VMs

[https://docs.microsoft.com/en-us/azure/virtual-machines/windows/download-template](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/download-template)

### Create connectivity between virtual networks

#### Create and configure VNET peering; create and configure VNET to VNET

* For peering Vnet should be in the same region
* Should not have overlapping IP Addresses
* No derived transitive relationship

[https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal)

#### Verify virtual network connectivity;

[https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#VerifyConnection](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#VerifyConnection)

[https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview)

#### Create a virtual network gateway

[https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpngateways](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpngateways)

### Implement and manage virtual networking

#### Configure private and public IP addresses

[https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-deploy-static-pip-arm-portal](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-deploy-static-pip-arm-portal)

[https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-static-private-ip-arm-pportal](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-static-private-ip-arm-pportal)

#### Network routes

[https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-udr-overview](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-udr-overview)

#### Configure network interfaces

[https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-network-interface-addresses](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-network-interface-addresses)

#### Configure subnets, and virtual network

[https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-manage-subnet](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-manage-subnet)

### Manage Azure Active Directory (AD)

#### Add custom domains
[https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/add-custom-domain](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/add-custom-domain)

#### configure Azure AD Identity Protection, Azure AD Join, and Enterprise State Roaming

[https://docs.microsoft.com/en-gb/azure/active-directory/identity-protection/enable](https://docs.microsoft.com/en-gb/azure/active-directory/identity-protection/enable)

[https://docs.microsoft.com/en-us/azure/active-directory/identity-protection/overview](https://docs.microsoft.com/en-us/azure/active-directory/identity-protection/overview)

[https://docs.microsoft.com/en-gb/azure/active-directory/user-help/user-help-join-device-on-network](https://docs.microsoft.com/en-gb/azure/active-directory/user-help/user-help-join-device-on-network)

[https://docs.microsoft.com/en-us/azure/active-directory/devices/enterprise-state-roaming-overview](https://docs.microsoft.com/en-us/azure/active-directory/devices/enterprise-state-roaming-overview)

#### configure self-service password reset

[https://docs.microsoft.com/en-gb/azure/active-directory/authentication/concept-sspr-howitworks](https://docs.microsoft.com/en-gb/azure/active-directory/authentication/concept-sspr-howitworks)

#### Implement conditional access policies

[https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/overview](https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/overview)

#### Manage multiple directories

[https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-administer#how-can-i-add-and-manage-multiple-directories](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-administer#how-can-i-add-and-manage-multiple-directories)

#### Perform an access review

[https://docs.microsoft.com/en-gb/azure/active-directory/governance/access-reviews-overview](https://docs.microsoft.com/en-gb/azure/active-directory/governance/access-reviews-overview)

### Implement and manage hybrid identities

#### Install and configure Azure AD Connect

[https://docs.microsoft.com/en-gb/azure/active-directory/hybrid/whatis-hybrid-identity](https://docs.microsoft.com/en-gb/azure/active-directory/hybrid/whatis-hybrid-identity)

#### Configure federation and single sign on
[https://docs.microsoft.com/en-us/azure/security/azure-ad-choose-authn](https://docs.microsoft.com/en-us/azure/security/azure-ad-choose-authn)

#### Manage AD connect

[https://docs.microsoft.com/en-gb/azure/active-directory/hybrid/how-to-connect-install-express](https://docs.microsoft.com/en-gb/azure/active-directory/hybrid/how-to-connect-install-express)

#### manage password sync and writeback

[https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-sspr-writeback](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-sspr-writeback)

## Implement Workloads and Security (20-25%)

### Migrate servers to Azure

#### Migrate by using ASR, Migrate using P2V, configure storage account

[https://docs.microsoft.com/en-us/azure/site-recovery/migrate-tutorial-on-premises-azure](https://docs.microsoft.com/en-us/azure/site-recovery/migrate-tutorial-on-premises-azure)

[https://channel9.msdn.com/Series/Azure-Site-Recovery/Hyper-V-to-Azure-with-ASR-Video1-Infrastructure-Setup](https://channel9.msdn.com/Series/Azure-Site-Recovery/Hyper-V-to-Azure-with-ASR-Video1-Infrastructure-Setup)

#### Create a backup vault

[https://docs.microsoft.com/en-us/azure/site-recovery/migrate-tutorial-on-premises-azure#create-a-recovery-services-vault](https://docs.microsoft.com/en-us/azure/site-recovery/migrate-tutorial-on-premises-azure#create-a-recovery-services-vault)

#### Prepare source and target environments

[https://docs.microsoft.com/en-us/azure/site-recovery/migrate-tutorial-on-premises-azure#set-up-the-source-environment](https://docs.microsoft.com/en-us/azure/site-recovery/migrate-tutorial-on-premises-azure#set-up-the-source-environment)

#### Backup and restore  data

[https://docs.microsoft.com/en-us/azure/backup/backup-azure-recovery-services-vault-overview](https://docs.microsoft.com/en-us/azure/backup/backup-azure-recovery-services-vault-overview)

#### Deploy Azure Site Recovery (ASR) agent

[https://docs.microsoft.com/en-us/azure/site-recovery/hyper-v-azure-tutorial#install-the-provider](https://docs.microsoft.com/en-us/azure/site-recovery/hyper-v-azure-tutorial#install-the-provider)

#### Prepare virtual network

[https://docs.microsoft.com/en-us/azure/site-recovery/tutorial-prepare-azure#set-up-an-azure-network](https://docs.microsoft.com/en-us/azure/site-recovery/tutorial-prepare-azure#set-up-an-azure-network)

### Configure Serverless Computing

#### Create and manage objects
[https://azure.microsoft.com/en-gb/overview/serverless-computing/](https://azure.microsoft.com/en-gb/overview/serverless-computing/)

#### Manage a logic app resource
[https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-overview](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-overview)
Do a little bit of hands-on
* Workflow oriented
* Trigger and Actions
* Ton of connectors including enterprise applications such as SAP
[https://docs.microsoft.com/en-us/azure/azure-functions/functions-compare-logic-apps-ms-flow-webjobs](https://docs.microsoft.com/en-us/azure/azure-functions/functions-compare-logic-apps-ms-flow-webjobs)

#### Manage Azure function app settings
[https://docs.microsoft.com/en-us/azure/azure-functions/functions-how-to-use-azure-function-app-settings](https://docs.microsoft.com/en-us/azure/azure-functions/functions-how-to-use-azure-function-app-settings)

#### Manage Eventgrid
https://docs.microsoft.com/en-us/azure/event-grid/overview
Webhook triggers need validation for custom endpoints through validation code or ValidationURL

#### Manage Servicebus
[https://docs.microsoft.com/en-gb/azure/service-bus-messaging/service-bus-messaging-overview](https://docs.microsoft.com/en-gb/azure/service-bus-messaging/service-bus-messaging-overview)
* Namespaces
* Queues
* Topics
* Message sessions
* Auto-forwarding
* Dead-Lettering
* Scheduled-delivery
* Message deferral
* Batching
* Transactions
* Filtering and actions
* Auto-delete on idle
* Duplication detection
* SAS, RBAC and MSI

[https://docs.microsoft.com/en-gb/azure/event-grid/compare-messaging-services?toc=%2fazure%2fservice-bus-messaging%2ftoc.json&bc=%2fazure%2fservice-bus-messaging%2fbreadcrumb%2ftoc.json](https://docs.microsoft.com/en-gb/azure/event-grid/compare-messaging-services?toc=%2fazure%2fservice-bus-messaging%2ftoc.json&bc=%2fazure%2fservice-bus-messaging%2fbreadcrumb%2ftoc.json)

### Implement application load balancing

#### Configure application gateway and load balancing rules

[https://docs.microsoft.com/en-us/azure/application-gateway/overview](https://docs.microsoft.com/en-us/azure/application-gateway/overview)

Level 7 Load balancer

* Autoscaling
* Static Virtual IP
* SSL Termination
* AKS Ingress
* Connection draining - Graceful removal of backend pools
* Custom error pages
* Web application firewall - OWASP 
* URL Based routing
* Multiple Site hosting
* Redirection (http - https)
* Session affinity
* Websocket and HTTP/2 traffic
* Rewrite HTTP Headers

#### Implement front end ip configurations

[https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-overview](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-overview)

Level 4 load balancer

Two types
* Internal Load balancer
* Public load balancer

#### Manage application load balancing

[https://docs.microsoft.com/en-us/azure/load-balancer/tutorial-load-balancer-standard-manage-portal](https://docs.microsoft.com/en-us/azure/load-balancer/tutorial-load-balancer-standard-manage-portal)

[https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-standard-overview](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-standard-overview)

### Integrate on premises network with Azure virtual network

#### Create and configure Azure VPN Gateway

[https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-plan-design](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-plan-design)

[https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpngateways](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpngateways)

#### Create and configure Site to Site VPN

[https://blogs.technet.microsoft.com/canitpro/2017/06/28/step-by-step-configuring-a-site-to-site-vpn-gateway-between-azure-and-on-premise/](https://blogs.technet.microsoft.com/canitpro/2017/06/28/step-by-step-configuring-a-site-to-site-vpn-gateway-between-azure-and-on-premise/)

[https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

#### Configure Express Route

[https://docs.microsoft.com/en-us/azure/expressroute/expressroute-introduction](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-introduction)

[https://docs.microsoft.com/en-us/azure/expressroute/expressroute-howto-circuit-portal-resource-manager](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-howto-circuit-portal-resource-manager)

#### Verify On-Premises connectivity

[https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

#### Manage on premise connectivity with azure

[https://docs.microsoft.com/en-us/office365/enterprise/connect-an-on-premises-network-to-a-microsoft-azure-virtual-network](https://docs.microsoft.com/en-us/office365/enterprise/connect-an-on-premises-network-to-a-microsoft-azure-virtual-network)

### Manage role-based access control (RBAC)

#### Create a custom role
[https://docs.microsoft.com/en-us/azure/role-based-access-control/custom-roles](https://docs.microsoft.com/en-us/azure/role-based-access-control/custom-roles)

#### Configure access to Azure resources by assigning roles

[https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-portal](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-portal)

#### Configure management access to Azure; Implement RBAC policies

[https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-admin-roles-secure](https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-admin-roles-secure)

[https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-portal#grant-access](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-portal#grant-access)

#### Troubleshoot RBAC

[https://docs.microsoft.com/en-us/azure/role-based-access-control/troubleshooting](https://docs.microsoft.com/en-us/azure/role-based-access-control/troubleshooting)

### Implement Multi-Factor authentication

#### Enable MFA for an Azure Tenant; configure user accounts for MFA

[https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-getstarted#enable-azure-multi-factor-authentication](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-getstarted#enable-azure-multi-factor-authentication)

[https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-userstates](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-userstates)

Disabled, Enabled and Enforced

#### configure fraud alerts

[https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-mfasettings#fraud-alert](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-mfasettings#fraud-alert)

#### configure bypass options

[https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-mfasettings#one-time-bypass](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-mfasettings#one-time-bypass)

#### Configure trusted Ips

[https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-mfasettings#trusted-ips](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-mfasettings#trusted-ips)
Trusted Ips only work on ipv4

Two types

* Managed : Specific range of IP address
* Federated : All federated users

#### Configure verification methods

[https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-mfasettings#mfa-service-settings](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-mfasettings#mfa-service-settings)

* Phone call
* SMS
* Notification APP
* Verification code from mobile app or hardware token

#### Repeated topics

* manage role-based access control (RBAC)
* implement RBAC policies
* assign RBAC Roles
* configure access to Azure resources by assigning roles
* configure management access to Azure

## Create and Deploy Apps (5-10%)

### Create web applications by using PaaS

This would definitely needs some hands-on experience.

#### Create an Azure app service web app by using Azure CLI, PowerShell, and other tools

[https://docs.microsoft.com/en-us/azure/app-service/samples-cli](https://docs.microsoft.com/en-us/azure/app-service/samples-cli)

[https://docs.microsoft.com/en-us/azure/app-service/samples-powershell](https://docs.microsoft.com/en-us/azure/app-service/samples-powershell)

[https://docs.microsoft.com/en-us/azure/app-service/scripts/cli-deploy-staging-environment](https://docs.microsoft.com/en-us/azure/app-service/scripts/cli-deploy-staging-environment)

#### create documentation for the API by using open source and other tools

[https://blogs.msdn.microsoft.com/appserviceteam/tag/swagger/](https://blogs.msdn.microsoft.com/appserviceteam/tag/swagger/)

[https://azure.microsoft.com/en-in/resources/videos/inside-web-apis-with-swagger-and-api-managment-with-brady-gaster/](https://azure.microsoft.com/en-in/resources/videos/inside-web-apis-with-swagger-and-api-managment-with-brady-gaster/)

#### create an App Service Web App for containers

[https://docs.microsoft.com/en-us/azure/app-service/containers/app-service-linux-intro](https://docs.microsoft.com/en-us/azure/app-service/containers/app-service-linux-intro)

#### create an App Service background task by using WebJobs

[https://docs.microsoft.com/en-us/azure/app-service/webjobs-create](https://docs.microsoft.com/en-us/azure/app-service/webjobs-create)

[https://docs.microsoft.com/en-us/azure/app-service/webjobs-sdk-get-started](https://docs.microsoft.com/en-us/azure/app-service/webjobs-sdk-get-started)

### Create app or service that runs on Service Fabric

I didn't go through much here as I have more than 2 years experience on service fabric.

* Develop a stateful Reliable Service and a stateless Reliable Service
* Develop an actor-based Reliable Service
* Write code to consume Reliable Collections in your service

[https://docs.microsoft.com/en-in/azure/service-fabric/service-fabric-overview](https://docs.microsoft.com/en-in/azure/service-fabric/service-fabric-overview)

[https://docs.microsoft.com/en-in/azure/service-fabric/service-fabric-architecture](https://docs.microsoft.com/en-in/azure/service-fabric/service-fabric-architecture)

[https://docs.microsoft.com/en-in/azure/service-fabric/service-fabric-reliable-actors-introduction](https://docs.microsoft.com/en-in/azure/service-fabric/service-fabric-reliable-actors-introduction)

[https://docs.microsoft.com/en-in/azure/service-fabric/service-fabric-reliable-services-reliable-collections](https://docs.microsoft.com/en-in/azure/service-fabric/service-fabric-reliable-services-reliable-collections)

### Design and develop applications that run in containers

#### Configure diagnostic settings on resources

[https://docs.microsoft.com/en-us/azure/azure-monitor/platform/diagnostic-logs-overview](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/diagnostic-logs-overview)

#### create a container image by using a Docker file

[https://blogs.msdn.microsoft.com/uk_faculty_connection/2016/09/23/getting-started-with-docker-and-container-services/](https://blogs.msdn.microsoft.com/uk_faculty_connection/2016/09/23/getting-started-with-docker-and-container-services/)

[https://docs.docker.com/docker-for-azure/deploy/](https://docs.docker.com/docker-for-azure/deploy/)

#### create an Azure Container Service (ACS/AKS) cluster by using the Azure CLI and Azure Portal

[https://stackify.com/azure-container-service-kubernetes/](https://stackify.com/azure-container-service-kubernetes/)

[https://docs.microsoft.com/en-gb/azure/aks/intro-kubernetes](https://docs.microsoft.com/en-gb/azure/aks/intro-kubernetes)

#### publish an image to the Azure Container Registry

[https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-docker-cli](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-docker-cli)

#### implement an application that runs on an Azure Container Instance

[https://docs.microsoft.com/en-us/azure/container-registry/container-registry-tutorial-quick-task](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-tutorial-quick-task)

[https://docs.microsoft.com/en-gb/azure/container-instances/container-instances-overview](https://docs.microsoft.com/en-gb/azure/container-instances/container-instances-overview)

[https://docs.microsoft.com/en-gb/azure/container-instances/container-instances-orchestrator-relationship](https://docs.microsoft.com/en-gb/azure/container-instances/container-instances-orchestrator-relationship)

#### implement container instances by using Azure Container Service (ACS/AKS), Azure Service Fabric, and other tools; manage container settings by using code

## Implement Authentication and Secure data (5-10%)

### Implement authentication

#### Implement authentication by using certificates, forms-based authentication, tokens, Windows-integrated authentication

[https://docs.microsoft.com/en-us/azure/app-service/overview-authentication-authorization](https://docs.microsoft.com/en-us/azure/app-service/overview-authentication-authorization)

[https://docs.microsoft.com/en-us/azure/active-directory/develop/authentication-scenarios](https://docs.microsoft.com/en-us/azure/active-directory/develop/authentication-scenarios)

#### Implement multi-factor authentication by using Azure AD options

[https://docs.microsoft.com/en-us/azure/active-directory/authentication/concept-mfa-howitworks](https://docs.microsoft.com/en-us/azure/active-directory/authentication/concept-mfa-howitworks)

### Implement secure data solutions

#### Encrypt and decrypt data at rest

[https://docs.microsoft.com/en-us/azure/security/azure-security-encryption-atrest](https://docs.microsoft.com/en-us/azure/security/azure-security-encryption-atrest)

[https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption-overview](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption-overview)

Disk backup is not enabled by default.

#### Encrypt data with Always Encrypted

[https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-2017](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-2017)

[https://docs.microsoft.com/en-us/azure/sql-database/sql-database-always-encrypted-azure-key-vault](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-always-encrypted-azure-key-vault)

#### Implement Azure Confidential Compute and SSL/TLS communications

[https://docs.microsoft.com/en-us/azure/security/azure-security-data-encryption-best-practices](https://docs.microsoft.com/en-us/azure/security/azure-security-data-encryption-best-practices)

[https://azure.microsoft.com/en-gb/blog/introducing-azure-confidential-computing/](https://azure.microsoft.com/en-gb/blog/introducing-azure-confidential-computing/)

#### Manage cryptographic keys in the Azure Key Vault

[https://docs.microsoft.com/en-us/azure/key-vault/key-vault-overview](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-overview)

[https://docs.microsoft.com/en-us/azure/key-vault/key-vault-key-rotation-log-monitoring](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-key-rotation-log-monitoring)

## Develop for Cloud (20-25%)

Again, I had skim through this topic as its mostly experienced based.

### Configure a message-based integration architecture

#### Configure an app or service to send emails, Event Grid, and the Azure Relay Service
[https://azure.microsoft.com/en-gb/blog/using-the-retry-pattern-to-make-your-cloud-application-more-resilient/](https://azure.microsoft.com/en-gb/blog/using-the-retry-pattern-to-make-your-cloud-application-more-resilient/)

#### Create and configure Notification Hub, Event Hub, and Service Bus

[https://docs.microsoft.com/en-us/azure/architecture/resiliency/](https://docs.microsoft.com/en-us/azure/architecture/resiliency/)

[https://docs.microsoft.com/en-us/azure/availability-zones/az-overview](https://docs.microsoft.com/en-us/azure/availability-zones/az-overview)

[https://docs.microsoft.com/en-us/azure/architecture/checklist/resiliency](https://docs.microsoft.com/en-us/azure/architecture/checklist/resiliency)

#### configure queries across multiple products

### Develop for autoscaling

#### Implement autoscaling rules and patterns (schedule, operational/system metrics, code that addresses singleton application instances)

#### Implement code that addresses transient state

[https://dzone.com/articles/azure-event-grid-webhooks-part-1](https://dzone.com/articles/azure-event-grid-webhooks-part-1)

[https://docs.microsoft.com/en-us/azure/automation/automation-webhooks](https://docs.microsoft.com/en-us/azure/automation/automation-webhooks)

Happy Coding !!!