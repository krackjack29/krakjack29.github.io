---
title: 'Executing remote scripts in Terraform template'
layout: single
comments: true
date : 2019/08/28
categories:
    - Terraform
tags:
    - gcp
    - cloud
    - iac
    - terraform
---

## What is Terraform?

According to Hashicorp (the creators of Terraform) it is. 

> Terraform is a tool for building, changing, and versioning infrastructure safely and efficiently. Terraform can manage existing and popular service providers as well as custom in-house solutions. Configuration files describe to Terraform the components needed to run a single application or your entire datacenter.

Terraform is a household name in the Infrastructure as Code circle, it supports all the major cloud vendors like Azure, AWS and GCP. It allows versioning of the infrastructure and also provides a single source of truth for you infrastructure configurations.

## Executing scripts
Terraform has a provider called "null_resource" which would be treated the same way any other provider is treated in-terms of lifecycle, but they do not provision any resource. 

These are used to run the scripts on the created resources, example would be 

```hcl
resource "null_resource" "deploy-pods" {
  triggers = {
    host = "${google_container_cluster.cluster.endpoint}"
    cluster_ca_certificate="${another.output}"
  }

  provisioner "local-exec" {
    command = <<EOT
     #Some command to be executed   
EOT
  }
}
```

**Triggers** are really important for the "null_resource" provider to execute the script in the "local-exec" block, triggers provide the neccesary state change. You could also run the scripts inside provisioned resource using "remote-exec" provisioner.

In the above example a deployment in the "template_file" yaml would be deployed to a k8s cluster, when the host endpoint of the K8s cluster or the CA certificate is modified/created, only then would the scripts be executed. 

Let's say in the second run only the template file is modified, terraform wouldn't run because the triggers wouldn't be altered. 

One way to trigger the scripts is by updating the trigger manual by adding a simple version number and incrementing it each time you would want to rerun the script.


```hcl
resource "null_resource" "deploy-pods" {
  triggers = {
    host = "${google_container_cluster.cluster.endpoint}"
    cluster_ca_certificate="${another.output}"
    version = "0.1"
  }

  provisioner "local-exec" {
    command = <<EOT
    #Some command to be executed
EOT
  }
}
```

Note: Ensure that you scripts are idempotent so that each time it executes, wouldn't result in error or scrambled state.

Happy Coding!!!