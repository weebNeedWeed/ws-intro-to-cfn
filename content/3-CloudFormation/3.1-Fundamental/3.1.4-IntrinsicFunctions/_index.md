---
title : "Intrinsic functions"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 3.1.4 </b> "
---

#### Overview

AWS CloudFormation provides you with some built-in functions called **Intrinsic Function** that help you manage your stack, values returned by calling those functions are not available until runtime. Without Intrinsic Function, you are limited to very basic templates, like the one in the **[3.1.2](/3-CloudFormation/3.1-Fundamental/3.1.2-SimpleStack)** chapter.

#### Ref

The **Ref** intrinsic function returns the value of the specified parameter or resource. In the last chapter, we have hard-coded the ImageId in the template, which is very troublesome for us to customize our stack with the other OS's ImageId. The solution here is to create a new parameter called *ImageId* and pass it to our instance.

```yaml
ImageId:
  Type: AWS::EC2::Image::Id
  Description: 'The ID of the Image.'
```

We define a *ImageId* parameter of type *AWS::EC2::Image::Id* then use **Ref** to pass it to the EC2 instance property.

```yaml
Resources:
  WebServerInstance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: !Ref InstanceType
      # Use !Ref function in ImageId property
      ImageId: !Ref ImageId
```

#### Fn::Join

**Fn::Join** is an Intrinsic Function that helps concatenating two or more strings with a delimiter between them. For example, you can use **`!Join [ "-", [ "value1", "value2" ] ]`** to get the **"value1-value2"** string.

How do we apply it to our template? The answer is very easy, we can utilize **Fn::Join** to assign tags to the resources defined in our template. Let's take a look at the illustration. 

```yaml
Resources:
  WebServerInstance:
    Type: AWS::EC2::Instance
    Properties:
      ImageId: !Ref ImageId
      InstanceType: !Ref InstanceType
      # Assign tag using Join
      Tags:
        - Key: Name
          Value: !Join [ '-', [ !Ref InstanceType, webserver ] ]
```

Once the EC2 instance is created, it is automatically assigned a tag with a key *Name* and a value *"t2.micro-webserver"* if you use t2.micro as instance type.

#### Fn::Sub

Another way to create a customized string is **Fn::Sub**. Sub stands for Substitute, it will replace variables in an input string with values that you specify. Fn::Sub is a bit harder than Fn::Join and Ref, so that we will learn the syntax of Fn::Sub before delving into examples.

```yaml
!Sub
  - String
  - Var1Name: Var1Value
    Var2Name: Var2Value
```
* **String**: The string containing variables in which you want to replace with your values.
* **VarName**: The name of a variable that you included in the String parameter.
* **VarValue**: The value that CloudFormation substitutes for the associated variable name at runtime.
  
For example:

```yaml
!Sub
  - ${InstanceType}
  - InstanceType: !Ref InstanceType
```
  
{{% notice tip %}}
If you include template parameter names or resource logical IDs, such as **${InstanceType}**, in the **String** parameter, CloudFormation returns the same values as if you used the Ref intrinsic function. That means, **!Ref InstanceType** is equal to **!Sub ${InstanceType}**.
{{% /notice %}}

Let's apply Fn::Sub to our template by assigning a new tag with Key *InstanceType* to the EC2 instance.

```yaml
Resources:
  WebServerInstance:
    Type: AWS::EC2::Instance
    Properties:
      ImageId: !Ref ImageId
      InstanceType: !Ref InstanceType
      Tags:
        - Key: Name
          Value: !Join [ '-', [ !Ref InstanceType, webserver ] ]
        - Key: InstanceType
          Value: !Sub ${InstanceType}
```

#### Reinforce your understanding

1\. Open Cloud9 then find **~/environment/ws2-material/workshop/fundamental/intrinsic-functions.yml**.

2\. Place the **ImageId** parameter below the InstanceType parameter.

```yaml
ImageId:
  Type: AWS::EC2::Image::Id
  Description: 'The ID of the Image.'
```

![Illustration](/images/3.1.4-IntrinsicFunctions/1.png)

3\. Replace the ImageId property.

```yaml
Resources:
  WebServerInstance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: !Ref InstanceType
      # Use !Ref function in ImageId property
      ImageId: !Ref ImageId
```

![Illustration](/images/3.1.4-IntrinsicFunctions/2.png)

4\. Add the Tags property to your instance.

```yaml
Resources:
  WebServerInstance:
    Type: AWS::EC2::Instance
    Properties:
      ImageId: !Ref ImageId
      InstanceType: !Ref InstanceType
      Tags:
        - Key: Name
          Value: !Join [ '-', [ !Ref InstanceType, webserver ] ]
        - Key: InstanceType
          Value: !Sub ${InstanceType}
```

![Illustration](/images/3.1.4-IntrinsicFunctions/3.png)

5\. Run the **create-stack** command to create a new stack.

```bash
cd ~/environment/ws2-material/workshop/fundamental
aws cloudformation create-stack --stack-name intrinsic-functions --template-body file://intrinsic-functions.yml --parameters ParameterKey=ImageId,ParameterValue="Replace with your ImageId"
```

![Illustration](/images/3.1.4-IntrinsicFunctions/4.png)

6\. Review the EC2 instance to confirm that it is created properly.

If there is no failure during the stack creation, you will obtain an EC2 instance like the instance in the image below.

![Illustration](/images/3.1.4-IntrinsicFunctions/5.png)

#### Cleaning up

**Run the delete command to delete your stack:**

```bash
aws cloudformation delete-stack --stack-name intrinsic-functions
```

#### Further readings

* **Other helpful intrinsic functions that you may need**:
  * **Fn::Base64**: Encode a string to its corresponding Base64 representation.
  * **Fn::Cidr**: returns an array of CIDR address block.
  * **Fn::GetAZs**: returns an array that lists Availability Zones for a specified Region.
  * **Fn::Length**: returns the number of elements in an array.
  * **Fn::Select**: returns a single object from a list of objects by index.
  * **Fn::Split**: splits the string into an array by the delimiter. Split is the opposite of Join.
  * **Fn::ToJsonString**: turns an object or an array into its JSON representation.
* **[Intrinsic functions](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference.html)**.