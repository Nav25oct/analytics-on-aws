---
title: "Prerequisites"
date: 2020-04-13T15:33:08-07:00
weight: 41
---

## General Requirements & Notes

* This workshop is self-paced and the instructions will guide you to achieve the goal of this workshop through AWS Management Console.
* While the lab lay down the steps to follow it is advised to take a moment to understand the purpose of each step. This will enhance the learning experience and help you relate the implementations you can have in your environment.

## Before you begin

For the sanity of the environment we will create new VPC to host the AWS Data Migration Service (DMS) replication instance. We will do this by using the AWS Cloudformation template.

The cloudformation template will perform the following:

* Create the required VPC for AWS Data Migration Service (DMS) replication instance
* Create Amazon Simple Storage Service (S3) bucket for destination endpoint configuration to be provided in AWS DMS
* Create required Amazon S3 bucket policies to allow AWS DMS to put data
* Create AWS Identity and Access Management (IAM) role to be used by AWS Glue later in the labs. ***This can be optional if the required AWS IAM role is already created with the same name.***

## Source MySQL

The source MySQL database details will be shared by the lab instructor. It should include the following details:

* mysql_host_name
* mysql_user_name
* mysq_user_password
* mysql_db_name

## Download cloudformation templates

* Use the following button to download the cloudformation template:

{{% button href="./DMSlab_student_role.json" %}}Download DMSlab_student_role.json{{% /button %}}

* You can also download the following cloudformation template. This will ne needed in case your account already has the IAM role created for Glue:

{{% button href="./DMSlab_student.json" %}}Download DMSlab_student.json{{% /button %}}


## Prerequisite Steps

### AWS Region

This lab is region independent however the recommendation is to run this lab in ***US East (N. Virginia) us-east-1***

### Create stack in AWS Cloudformation

* Access AWS Management Console where you plan to run this workshop and navigate to *AWS CloudFormation*
* Click on ***"Create stack"***, under "Specify tempate" select the radio button ***"Upload a template file"***
* Click ***"Chose file"*** to access and browse your file system. Locate the cloudformation template json file "DMSlab_student_role.json" downloaded earlier and click "Next"
* Provide an identifiable stack name under "Stack name" and click "Next"
* Accept the default configuration on next screen and click "Next". On the final screen scroll all the way to the bottom of the page and select the checkbox in the "Capabilities" section to acknowledge IAM resouce creation and then click on ***"Create stack"***.
* The stack creation should take up to 5 minutes
* Once the stack creation is complete click on the ***"Outputs"*** tab and make a note of GlueLabRole, DMSLabRoleS3 and BucketName. We will need these values later in the lab.


Congratulations! You have completed the prerequisites steps needed to continue with the workshop. You should have a new VPC with the required subnet configuration, S3 bucket and IAM roles. You can continue to the next steps in the workshop.


* Login to 
