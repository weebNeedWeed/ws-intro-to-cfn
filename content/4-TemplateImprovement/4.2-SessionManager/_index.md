---
title : "Session Manager"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 4.2 </b> "
---

#### Overview

**[Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html)** is a fully managed System Manager feature that allows you to manage your EC2 instances via an interactive one-click browser-based terminal or through the AWS CLI.

Session Manager offers some benefits over using SSH:

1. No open inbound ports in the Security Group.
2. No need to manage bastion hosts or SSH keys.
3. Centralized access control to managed nodes using IAM policies.
4. Commands and activities can be logged to S3 or CloudWatch, facilitating auditing.
5. One-click access to managed nodes from the AWS Manager Console and the AWS CLI.

#### How Session Manager works

1. User authenticates against IAM.
2. IAM authorizes to start a session on an EC2 instance by evaluating applicable IAM policies.
3. User uses the AWS Management Console and the terminal (AWS CLI and additional plugins required) to start a session via System Manager.
4. The System Manager agent running on the instance connect to the System Manager service and execute commands on the instance.
5. The Session Manager sends audit logs to CloudWatch or S3.

{{% notice note %}}
For Session Manager to work, the EC2 instance must have access to the Internet, or VPC Endpoints.
{{% /notice %}}

![Architecture](/images/4.2-SessionManager/session-manager.png)

#### Hands-on lab

This part will guide you through several steps required to start a session on an EC2 instance which has no SSH port open. The instance still has the public IP address for communicating with the Internet.

1\. Open Cloud9 environment, search for **~/environment/ws2-material/workshop/fundamental/session-manager.yml**.

2\. To use System Manager, we need to provision an IAM Role which contains the **AmazonSSMManagedInstanceCore** policy.

```yaml
  IAMRole:
    Type: 'AWS::IAM::Role'
    Properties:
      AssumeRolePolicyDocument:
        Version: "2012-10-17"
        Statement:
          - Effect: Allow
            Principal:
              Service:
                - ec2.amazonaws.com
            Action:
              - 'sts:AssumeRole'
      ManagedPolicyArns:
        - "arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore"
```

![Illus](/images/4.2-SessionManager/1.png)

3\. Create a new Instance Profile with IAM Role attached.

```yaml
  IAMInstanceProfile: 
    Type: "AWS::IAM::InstanceProfile"
    Properties: 
      Roles: 
        - !Ref IAMRole
```

![Illus](/images/4.2-SessionManager/2.png)

4\. Update the EC2 instance to reference the Instance Profile.

```yaml
  WebServerInstance:
    Type: AWS::EC2::Instance
    Properties:
      IamInstanceProfile: !Ref IAMInstanceProfile
      ImageId: !Ref AmiID
      InstanceType: t2.micro
```

![Illus](/images/4.2-SessionManager/3.png)

5\. The preparation step has been done, next the stack creation. Execute the command below to create a new stack.

```bash
cd ~/environment/ws2-material/workshop/fundamental
aws cloudformation create-stack --stack-name session-manager --template-body file://session-manager.yml --capabilities CAPABILITY_IAM
```

6\. Wait for around **5 minutes**. Then on the AWS Management Console, search for `Session Manager`.

![Illus](/images/4.2-SessionManager/4.png)

7\. Click on **Start session**.

![Illus](/images/4.2-SessionManager/5.png)

8\. Select the EC2 instance provisioned via CloudFormation. Click on **Start session**.

![Illus](/images/4.2-SessionManager/6.png)

9\. Once you have completed your work, you can click on **Terminate** to end the session.

![Illus](/images/4.2-SessionManager/7.png)

#### Cleaning up

**Run the delete command to delete your stack:**

```bash
aws cloudformation delete-stack --stack-name session-manager
```

#### Further readings

* **[AWS Systems Manager Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html)**.