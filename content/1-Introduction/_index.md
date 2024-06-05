---
title : "Introduction"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---

#### What is Infrastructure as Code ?

Infrastructure as Code or IaC is the practice of managing and provisioning cloud computing infrastructure(such as virtual machines, storage or networks) through machine-readable definition files, rather than configuring resources through a user interface or traditional scripting.

The key benefit of IaC is that you can achieve **consistency** and **repeatability** when creating and configuring infrastructure. IaC ensures that the infrastructure is provisioned consistently across various environment(Dev, Prod or Staging, etc...), reducing the risk of configuration drift and enabling reliable and predictable deployments.

#### What is AWS CloudFormation ?

**AWS CloudFormation** is a Infrastructure as Code tool provided by AWS that enables you to define, manage and provision AWS resources using declarative templates. These template can be written in YAML or JSON format and define a desired state of your infrastructure stack.

#### What is Terraform ?

**Terraform** is an open-source Infrastructure as Code tool created by HashiCorp that enables you to provision a wide range of infrastructure services (such as virtual machines, databases, networks, etc...) across **multiple cloud providers(AWS, GCP and Azure)** or **on-premise environments** through a simple and declarative template.

In Terraform, you can generate resources across multiple environments while CloudFormation can only provision resources on the AWS environment.