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

3. You should be able to see tpc-database under instance, this is the RDS instance which was created by AWS CloudFormation template. When you select that database, Database name and Username fields will be populated automatically. Enter the database user password (default: BigData26!) in the Password field, and click Next.

![glueconnection3](/image/glueconnection3.png)

4. Next window will show all connection information that you provided, verify that information, and click **Finish**.
5. Verify the newly created connection from the connection list, select that connect using the checkbox and click on Test connection to check the connection.

![glueconnection4](/image/glueconnection4.png)

6. Select IAM role you have created in **prerequisite** section from the dropdown and click Test connection.