---
title: 'AZ-301 Microsoft Azure Architect Design Certification'
layout: single
comments: true
date : 2019/02/19
categories:
    - Azure
tags:
    - azure
    - cloud
    - certifications
---

I completed AZ-301 Microsoft Azure Architect Design exam successfully, which earned me **Azure Solution Architect Expert** certification. 

<div data-iframe-width="150" data-iframe-height="270" data-share-badge-id="24e8cb5b-1913-45ff-8ae1-99a66ed974d3"></div><script type="text/javascript" async src="//cdn.youracclaim.com/assets/utilities/embed.js"></script>

I had cleared [AZ-300]({% post_url 2019-02-05-az-300-azure-architect-certification %}) couple of weeks back. Both these exams are required to get the certification. The exam was mostly case-study and scenario-based, which was much required in similar lines as AWS professional certification. The exam covers all the services in Azure, but mostly covers Azure Active Directory, Site Recovery for migration and Networking.  

I would recommend for the aspiring fellow Azure professionals to study the following components

* **Networking**
  * Concentrate on Hybrid connectivity scenarios using Site-Site VPN, Point-Site VPN and ExpressRoute.
  * Ensuring Connectivity from disaster-recovery perspective. 
  * Monitoring using Network watcher.
  
* **Azure Active Directory**
  * Migrating/connecting the on-premise Active Directory forests with the Azure AD. 
  * Different Authentication and Authorization supported by Azure AD.
  * Functionalities provided by Premium version of Azure AD such as PIM, MFA, Access Policy etc.
  
* **Migration**
  * Migrating servers using Site Recovery.
  * Identifying services based on various factors of workloads. 
  * Lift and Shift, Refactoring and Rewriting etc.

* **Monitoring**
  * Services such as Cost management and Billing.
  * Network watcher, App Insights and Log analytics.
  * Alert rules etc.

If you are new to Azure and want to get this certification, I would recommend 57-hour tutorial videos at [Pluralsight](https://app.pluralsight.com/paths/certificate/microsoft-azure-architect-design-az-301), it's really insightful even if you are not preparing for the exam.

Below is the full syllabus for the exam.

## Determine Workload and Requirements (10-15%)
### Gather Information and Requirements
* Identify compliance requirements, identity and access management infrastructure, and service-oriented architectures (e.g., integration patterns, service design, service discoverability); 
* Identify accessibility (e.g. Web Content Accessibility Guidelines), availability (e.g. Service Level Agreement), capacity planning and scalability, deploy-ability (e.g., repositories, failback, slot-based deployment), configurability, governance, maintainability (e.g. logging, debugging, troubleshooting, recovery, training), security (e.g. authentication, authorization, attacks), and sizing (e.g. support costs, optimization) requirements; 
* Recommend changes during project execution (ongoing); 
* Evaluate products and services to align with solution; create testing scenarios

### Optimize Consumption Strategy
* Optimize app service, compute, identity, network, and storage costs

### Design an Auditing and Monitoring Strategy
* Define logical groupings (tags) for resources to be monitored; 
* determine levels and storage locations for logs; 
* plan for integration with monitoring tools; 
* recommend appropriate monitoring tool(s) for a solution; 
* specify mechanism for event routing and escalation; 
* design auditing for compliance requirements; 
* design auditing policies and traceability requirements

## Design For Identity and Security (20-25%)
### Design Identity Management
* Choose an identity management approach; 
* design an identity delegation strategy, identity repository (including directory, application, systems, etc.); 
* design self-service identity management and user and persona provisioning; 
* define personas and roles; 
* recommend appropriate access control strategy (e.g., attribute-based, discretionary access, history-based, identity-based, mandatory, organization-based, role-based, rule-based, responsibility-based)

### Design Authentication
* Choose an authentication approach; 
* design a single-sign on approach; 
* design for IPSec, logon, multi-factor, network access, and remote authentication

### Design Authorization
* Choose an authorization approach; 
* define access permissions and privileges; 
* design secure delegated access (e.g., oAuth, OpenID, etc.); 
* recommend when and how to use API Keys.

### Design for Risk Prevention for Identity
* Design a risk assessment strategy (e.g., access reviews, RBAC policies, physical access); 
* evaluate agreements involving services or products from vendors and contractors; 
* update solution design to address and mitigate changes to existing security policies, standards, guidelines and procedures

### Design a Monitoring Strategy for Identity and Security
* Design for alert notifications; 
* design an alert and metrics strategy; 
* recommend authentication monitors

## Design a Data Platform Strategy (15-20%)
### Design a Data Management Strategy
* Choose between managed and unmanaged data store; 
* choose between relational and non-relational databases; 
* design data auditing and caching strategies; 
* identify data attributes (e.g., relevancy, structure, frequency, size, durability, etc.); 
* recommend Database Transaction Unit (DTU) sizing; 
* design a data retention policy; design for data availability, consistency, and durability; 
* design a data warehouse strategy

### Design a Data Protection Strategy
* Recommend geographic data storage; 
* design an encryption strategy for data at rest, for data in transmission, and for data in use; 
* design a scalability strategy for data; 
* design secure access to data; 
* design a data loss prevention (DLP) policy

### Design and Document Data Flows
* Identify data flow requirements; 
* create a data flow diagram; 
* design a data flow to meet business requirements; 
* design a data import and export strategy

### Design a Monitoring Strategy for the Data Platform
* Design for alert notifications; 
* design an alert and metrics strategy

## Design a Business Continuity Strategy (15-20%)
### Design a Site Recovery Strategy
* Design a recovery solution; 
* design a site recovery replication policy; 
* design for site recovery capacity and for storage replication; 
* design site failover and failback (planned/unplanned); 
* design the site recovery network; 
* recommend recovery objectives (e.g., Azure, on-prem, hybrid, Recovery Time Objective (RTO), Recovery Level Objective (RLO), Recovery Point Objective (RPO)); 
* identify resources that require site recovery; 
* identify supported and unsupported workloads; 
* recommend a geographical distribution strategy

### Design for High Availability
* Design for application redundancy, autoscaling, data center and fault domain redundancy, and network redundancy; 
* identify resources that require high availability; 
* identify storage types for high availability

### Design a disaster recovery strategy for individual workloads
* Design failover/failback scenario(s); 
* document recovery requirements; 
* identify resources that require backup; 
* recommend a geographic availability strategy

### Design a Data Archiving Strategy
* Recommend storage types and methodology for data archiving; 
* identify requirements for data archiving and business compliance requirements for data archiving; 
* identify SLA(s) for data archiving

## Design for Deployment Migration and Integration (10-15%)
### Design Deployments
* Design a compute, container, data platform, messaging solution, storage, and web app and service deployment strategy

### Design Migrations
* Recommend a migration strategy; 
* design data import/export strategies during migration; 
* determine the appropriate application migration, data transfer, and network connectivity method; 
* determine migration scope, including redundant, related, trivial, and outdated data; 
* determine application and data compatibility

### Design an API Integration Strategy
* Design an API gateway strategy; 
* determine policies for internal and external consumption of APIs; 
* recommend a hosting structure for API management

## Design an Infrastructure Strategy (15-20%)
### Design a Storage Strategy
* Design a storage provisioning strategy; 
* design storage access strategy; 
* identify storage requirements; 
* recommend a storage solution and storage management tools

### Design a Compute Strategy
* Design compute provisioning and secure compute strategies; 
* determine appropriate compute technologies (e.g., virtual machines, functions, service fabric, container instances, etc.); 
* design an Azure HPC environment; 
* identify compute requirements; 
* recommend management tools for compute

### Design a Networking Strategy
* Design network provisioning and network security strategies; 
* determine appropriate network connectivity technologies; 
* identify networking requirements; 
* recommend network management tools

### Design a Monitoring Strategy for Infrastructure
* Design for alert notifications; 
* design an alert and metrics strategy

Happy coding!!!