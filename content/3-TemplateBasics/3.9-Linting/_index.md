---
title : "Linting"
date : "`r Sys.Date()`"
weight : 9
chapter : false
pre : " <b> 3.9 </b> "
---

#### Overview

Linter is a tool that helps improving your template. It scans your template for errors, identifies them, and then alerts you so you can fix them before putting your template into production.

When working with CloudFormation, we typically use the linter written in Python called **cfn-lint**, which helps analyze our templates and warn us about potential errors such as circular references, YAML syntax, and spelling mistakes.

Let's see how we can install it.

#### Cfn-lint installation

1\. In the Cloud9 environment, open the terminal and run that command to install **cfn-lint**.

```bash
pip install cfn-lint
```

2\. Verify cfn-lint's version, confirming it is installed successfully.

```bash
cfn-lint --version
```

![Illustration](/images/3.9-Linting/1.png)

#### Fixing the template using cfn-lint

1\. Open Cloud9 then find **~/environment/ws2-material/workshop/fundamental/linting.yml**.

In this lab, we provide you with an error template so you can practicing cfn-lint through it.

2\. Run **cfn-lint** against the template.

```bash
cd ~/environment/ws2-material/workshop/fundamental
cfn-lint linting.yml
```

This will show you two errors, the first one says that your template contains a circular reference with **SecurityGroup**, the second one says that you target the wrong vpc.

![Illustration](/images/3.9-Linting/2.png)

3\. Examining the first error, you will find that inside **SecurityGroupIngress**, there is one **!Ref** to **SecurityGroup**, which is the parent resource containing **SecurityGroupIngress**. The reference like that is not allowed in CloudFormation.

![Illustration](/images/3.9-Linting/3.png)

To fix that, we will simply turn **SecurityGroupIngress** into a separated resource, then add a reference to **SecurityGroup**.

```yaml
  SecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Example Security Group
      SecurityGroupEgress:
        - Description: Example rule limiting egress traffic to 127.0.0.1/32
          CidrIp: 127.0.0.1/32
          IpProtocol: "-1"
      VpcId: !Ref AnotherVpc

  SecurityGroupIngress:
    Type: AWS::EC2::SecurityGroupIngress
    Properties:
      Description: Example rule to allow tcp/443 traffic from SecurityGroup
      FromPort: 443
      ToPort: 443
      GroupId: !Ref SecurityGroup
      IpProtocol: tcp
      SourceSecurityGroupId: !Ref SecurityGroup
```

![Illustration](/images/3.9-Linting/4.png)

4\. Revalidate the template.

```bash
cfn-lint linting.yml
```

![Illustration](/images/3.9-Linting/5.png)

It works! We solve the reference problem.

5\. To fix the second error, the wrong vpc, we simply reference the correct VPC.

![Illustration](/images/3.9-Linting/6.png)

Then validate the template again. The command execution shows no errors, telling that we can now deploy our template.

![Illustration](/images/3.9-Linting/7.png)

6\. Execute command to deploy the template.

```bash
aws cloudformation create-stack --stack-name linting --template-body file://linting.yml
```

7\. Validate our stack is successfully created.

![Illustration](/images/3.9-Linting/8.png)

#### Cleaning up

**Run the delete command to delete your stack:**

```bash
aws cloudformation delete-stack --stack-name linting
```

#### Further readings

* **[cfn-lint documentation](https://github.com/aws-cloudformation/cfn-lint)**.