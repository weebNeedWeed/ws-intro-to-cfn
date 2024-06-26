---
title : "Mappings"
date : "`r Sys.Date()`"
weight : 8
chapter : false
pre : " <b> 3.8 </b> "
---

#### What is Mappings ?

The **Mappings** is used to define a map which matches a key to corresponding set of named values. Please check the image below to have an overview of Mappings.

![Illustration](/images/3.8-Mappings/1.png)

To retrieve a value of a map, we will use the **Fn::FindInMap** intrinsic function. The short form syntax of it is:

```yaml
!FindInMap [ MapName, TopLevelKey, SecondLevelKey ]
```

Here is an example of a simplified map. It contains a map called **EnvironmentToInstanceType**.Within it, we define two top level keys, **Test** and **Prod**. And each top level key has a **InstanceType: \<type\>** pairs. Then, we reference it in the WebServerInstance using the **FindInMap** instance.

```yaml
Mappings:
  EnvironmentToInstanceType: # Map Name
    Test: # Top level key
      InstanceType: t2.micro # Second level key
    Prod:
      InstanceType: t2.small

Resources:
  WebServerInstance:
    Type: AWS::EC2::Instance
    Properties:
      ImageId: !Ref AmiID
      InstanceType: !FindInMap
        - EnvironmentToInstanceType # Map Name
        - Test # Top Level Key
        - InstanceType # Second Level Key
```

#### Let's take a lab

1\. Open Cloud9 then find **~/environment/ws2-material/workshop/fundamental/mappings.yml**.