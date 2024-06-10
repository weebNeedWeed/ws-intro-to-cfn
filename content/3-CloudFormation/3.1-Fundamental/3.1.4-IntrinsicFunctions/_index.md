---
title : "Intrinsic functions"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 3.1.4 </b> "
---

#### Overview

AWS CloudFormation provides you with some built-in functions called **Intrinsic Function** that help you manage your stack, values returned by calling them are not available until runtime. Without Intrinsic Function, you are limited to very basic templates, like the one in the **[3.1.2](/3-CloudFormation/3.1-Fundamental/3.1.2-SimpleStack)** chapter.

#### Ref

The intrinsic function **Ref** returns the value of the specified parameter or resource. In the last chapter, we have hard-coded the ImageId in the template, which is very troublesome for us to create a stack but with the other OS's ImageId. The solution here is to create a new parameter called *ImageId* and pass it to our instance.

```yaml
ImageId:
  Type: AWS::EC2::Image::Id
  Description: 'The ID of the Image.'
```

We define a *ImageId* parameter of type *AWS::EC2::Image::Id* then we can use **Ref** to pass it to the EC2 instance property.

```yaml
Resources:
  WebServerInstance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: !Ref InstanceType
      # Use !Ref function in ImageId property
      ImageId: !Ref ImageId
```