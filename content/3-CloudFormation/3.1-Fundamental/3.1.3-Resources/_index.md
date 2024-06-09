---
title : "Metadata, Parameters and Resources"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3.1.3 </b> "
---

#### Metadata

The **Metadata** section allows you to define additional information about your template through the arbitrary JSON or YAML object. Let's take a look at the example below. In this example, we define two custom fields, *Instances* and *Databases*, which provide information about EC2 instances and databases in our stack.

```yaml
Metadata:
  Instances:
    Description: "Information about the instances"
  Databases: 
    Description: "Information about the databases"
```

Moreover, in the **Metadata** section, we can improve the user experience(UX) of deployment by grouping, sorting and providing informative description for parameters in the template. All of those can be done using the **AWS::CloudFormation::Interface** key in Metadata.

```yaml
Metadata:
  AWS::CloudFormation::Interface:
    ParameterGroups:
      - Label: LabelOfParameterGroup1
        Parameters:
          - Param1
          - Param2
    ParameterLabels:
      Param1:
        default: Param1Label
      Param2:
        default: Param2Label
```

The illustration above gives you one group with two parameters. When viewing the template on the AWS console, you will see that two parameters are in the group that has the label *LabelOfParameterGroup1* and they are both designated as *ParamXLabel*. 

#### Parameters

Parameters enable you to input custom values to your template each time you create or update a stack. Let's examine the example below:

```yaml
Parameters:
  InstanceType:
    Type: String
    Default: t2.micro
    AllowedValues:
      - t2.micro
      - m1.small
      - m1.large
    Description: Enter t2.micro, m1.small, or m1.large. Default is t2.micro.
```

As you can see, we have the *InstanceType* parameter which accepts a string value and is constrained by three values: t2.micro, m1.small and m1.large. You can pass in this parameter to build appropriate instance when creating or updating your stack.

