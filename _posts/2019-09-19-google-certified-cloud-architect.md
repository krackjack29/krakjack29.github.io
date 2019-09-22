---
title: 'Google Certified Cloud Architect Professional'
header: 
    image: /assets/images/certificates/gcp-cert.png
    caption: "GCP Certification"
comments: true
date : 2019/09/19
categories:
    - GCP
tags:
    - gcp
    - cloud
    - certifications
---

I recently cleared the Google Certified Professional Cloud Architect exam and hereby I am sharing the list of study materials and the concepts of the GCP that the certification concentrates on. 

I was part of the Partner program by google and got access to the following [Coursera](https://coursera.org) courses.

1. *Google Cloud Platform Fundamentals: Core Infrastructure*
2. *Essential Cloud Infrastructure: Foundation*
3. *Essential Cloud Infrastructure: Core Services*
4. *Elastic Cloud Infrastructure: Scaling and Automation*
5. *Elastic Cloud Infrastructure: Containers and Services*
6. *Reliable Cloud Infrastructure: Design and Process*

These courses are designed to give you an overview of all the services in GCP except for Bigquery, Bigtable and other AI/ML services.

The partner program also gave credits to [Qwiklabs](https://qwiklabs.com), which actually provides a hands-on lab environment to explore and play with GCP services. The environments are designed to give a step by step instructions to perform tasks like "hosting a scale-able web application using instance groups".

## Topics covered in the Exam

* Compute Engine
   1. Instance groups and scaling
   2. Images and Snapshots
* Google Kubernetes Engine
   1. Basic concepts and Commands
* Networking
   1. VPC
   2. Hybrid connection scenarios
   3. Load balancing
   4. Firewalls
* Stackdriver : Everything about stackdriver
* Cloud Agnostic design concepts:
   1. Scaling
   2. Availability
   3. Disaster Recovery
   4. Resiliency 
* Casestudies : Exam has 4 case studies out of which 3 of them are already listed in the website
   1. [Mountkirk Games](https://cloud.google.com/certification/guides/cloud-architect/casestudy-mountkirkgames-rev2)
   2. [Dress4Win](https://cloud.google.com/certification/guides/cloud-architect/casestudy-dress4win-rev2/)
   3. [TerramEarth](https://cloud.google.com/certification/guides/cloud-architect/casestudy-terramearth-rev2/)
* BigTable & BigQuery : There were a lot of questions on Bigtable and Bigquery especially on the costing and storage for logging. 
   1. Time-series data for Bigtable. [https://cloud.google.com/bigtable/docs/schema-design-time-series](https://cloud.google.com/bigtable/docs/schema-design-time-series)
   2. Schema Design for Bigtable. [https://cloud.google.com/bigtable/docs/schema-design](https://cloud.google.com/bigtable/docs/schema-design)
   3. Bigquery time partitioning tables. [https://cloud.google.com/bigquery/docs/partitioned-tables](https://cloud.google.com/bigquery/docs/partitioned-tables)
   4. Bigquery cost optimization. [https://cloud.google.com/bigquery/docs/best-practices-costs](https://cloud.google.com/bigquery/docs/best-practices-costs)
   5. Bigquery data loading [https://cloud.google.com/bigquery/docs/loading-data](https://cloud.google.com/bigquery/docs/loading-data)
* Organization, Folder and Project hierarchy.

## External resources
Along with the courses, documentation and Qwiklabs there was also help from fellow techies, who shared there study notes with the world.

* [https://medium.com/@agasthi.kothurkar/google-certified-professional-cloud-architect-study-resources-a66f8f52aac5](https://medium.com/@agasthi.kothurkar/google-certified-professional-cloud-architect-study-resources-a66f8f52aac5)
* [https://medium.com/@earlg3/google-cloud-architect-exam-study-materials-5ab327b62bc8](https://medium.com/@earlg3/google-cloud-architect-exam-study-materials-5ab327b62bc8)
* [https://medium.com/@sathishvj/notes-from-my-google-cloud-professional-cloud-architect-exam-bbc4299ac30](https://medium.com/@sathishvj/notes-from-my-google-cloud-professional-cloud-architect-exam-bbc4299ac30)
* [https://medium.com/@agasthi.kothurkar/google-certified-professional-cloud-architect-study-resources-a66f8f52aac5](https://medium.com/@agasthi.kothurkar/google-certified-professional-cloud-architect-study-resources-a66f8f52aac5)
* [https://medium.com/google-cloud/professional-cloud-architect-certification-6a6dfa5c6ff5](https://medium.com/google-cloud/professional-cloud-architect-certification-6a6dfa5c6ff5)

Overall it took me about 6 weeks of study and was fortunate to work on a highly complex GCP project for the same duration, I was able to clear it in a single attempt.

Happy Coding!!!