---
title: "Connection"
chapter: true
weight: 2
---


The CloudFormation template in the Prerequisite section created a temporary database in RDS with TPC data. In this exercise, you will create a Glue Connection to connect to that RDS database.


1. Click on the Connections option on the left and then click on Add connection button.

![glueconnection1](/image/glueconnection1.png)

2. In this section, you will provide connection details. Enter **mysql-connector** as the **Connection Name**, select **Amazon RDS** from the **Connection type** and **MySQL**as the **Database engine**, and click Next.

![glueconnection2](/image/glueconnection2.png)