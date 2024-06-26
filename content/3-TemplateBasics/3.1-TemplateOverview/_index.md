---
title : "Template overview"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---

#### What is the CloudFormation template ?

A template is a text file that serves as a blueprint for your infrastructure, defining the resources ,their configuration and dependencies. When you create or update a **[stack](#what-is-the-cloudformation-stack-)**, CloudFormation provisions and configures the resources specified in the template.

So let we discuss the common concepts of the CloudFormation template:

1. **AWSTemplateFormatVersion (optional):** identifies the capabilities of the template. The current only accepted value is **"2010-09-09"**.

2. **Description (optional):** enables you to include comments about your template.

3. **Metadata (optional):** enables you to include additional information about the template or resources, such as resources description or configuration settings.

4. **Parameters (optional):** defines parameters that can be used to customize resources when creating or updating the stack.

5. **Rules (Optional):** validates parameters passed in during a stack creation or stack update.

6. **Mappings (Optional):** matches a key to a corresponding set of named values.

7. **Conditions (Optional):** defines conditional expressions that can be used to control the creation and update of resources based on certain criteria or parameter values.

8. **Transform (Optional):** defines one or more macros(A reusable piece of CloudFormation configuration) that CloudFormation should use to process before creating or updating the stack.

9. **Resources *(REQUIRED)*:** declares the AWS resources that you want to create.

10. **Outputs (Optional):** defines the output values that can be returned or referenced by other stacks.

#### What is the CloudFormation stack ?

A stack is representation of collections of AWS resources that you can manage as a single unit. A stack is created based on a template.

CloudFormation will create, update and delete a stack in its entirety. When any failure in the stack creation or stack update, CloudFormation will roll it back to the previous state, keeping it always in a consistent state and reducing the risk of manual error. 

![Stack architecture](/images/3.1-Overview/cfn-stack.png)

#### Further readings

* **[Template anatomy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html)**.