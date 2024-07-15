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

#### Content

1. [Introduction](1-introduce/)
2. [Preparation](2-firewallinvpc/)
3. [Template Basics](3-TemplateBasics)
4. [Template Improvement](4-TemplateImprovement)
5. [Conclusion](5-Conclusion)