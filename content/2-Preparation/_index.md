---
title : "Preparation"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

#### Overview

In order to reinforce the understanding of the material covered in this lab, you will set up a Cloud9 instance, a managed IDE that lets you work directly in your browser, and put your newly learned IaC skills into practice.

This section will guide you creating a Cloud9 environment through some simple steps. In that environment, you will be able to access necessary tools for fulfilling this lab such as AWSCLI, python, etc...

#### A quick review of YAML

AWS CloudFormation use YAML and JSON as the configuration language. In this workshop, you will mainly use YAML to write the templates, so please make sure that you take a cursory look at the **[YAML Cheat Sheet](https://quickref.me/yaml.html)**.

#### Creating an environment

1. Log in to the console, search for ```Cloud9```.

![1](/images/2-Preparation/1.png)

2. Click **My environments**, then click **Create environment**.

![2](/images/2-Preparation/2.png)

3. Fill in **Name** as ```cfn-workshop```.

![3](/images/2-Preparation/3.png)

4. Scroll down and click **Create**.

![4](/images/2-Preparation/4.png)

5. Wait for the environment successfully creating, then click **Open**.

![5](/images/2-Preparation/5.png)

6. In the terminal, execute the command below to pull down the material for this lab.

```bash
git clone https://github.com/weebNeedWeed/ws2-material
```

![6](/images/2-Preparation/6.png)

7. Successfully creating the lab environment.

![7](/images/2-Preparation/7.png)
