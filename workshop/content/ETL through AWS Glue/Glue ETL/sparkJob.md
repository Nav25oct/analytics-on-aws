---
title: "Spark Job"
chapter: true
---

An Apache Spark ETL job consists of the business logic that performs ETL work in AWS Glue. You can monitor job runs to understand runtime metrics such as success, duration, and start time. The output of a job is your transformed data, written to a location that you specify.

In the previous step, you crawled S3 TPC-DS text file and discovered the schema of TPC-DS data. In this exercise, you will create a Spark job in Glue, which will read that source, write them to S3 in Parquet format.


1. Click on the Jobs option on the left and then click on Add job button.

![gluejob1](/image/gluejob1.png)


2. Give a name for your job **tpcds-text-to-parquet**, Click on Create IAM Role and create new IAM Role called ***tpcds-text-to-parquet-role*** with 

Here are steps you can follow to create IAM Role 

Create IAM Role for Glue

* Login to the AWS console, click the drop down on “Services” and type “IAM” in the search bar.

* Choose “Roles” in the IAM console and click on the tab “Create role” 

* "Select type of trusted entity" as "AWS Service" and "Choose the service that will use this role" as "Glue"

![gluejob2](/image/glueIAM1.png)



![gluejob2](/image/glueSparkJobRole.png)


3. From the IAM role drop-down list (Refresh it; if it not showing) - select ***tpcds-text-to-parquet-role***. 

![gluejob3](/image/glueIamRole.png)

4. Since you are creating Spark ETL job, so leave the rest of the fields as default.

Note - If you able to see new Glue 2.0 Version from drop-down in Glue Version; select it. 

![gluejob4](/image/sparkglue20.png)

5. select table which you want to convert to parquete - Hence in this example we are selecting **reason**

![gluejob4](/image/gluejob5.png)


6. Select **Change Schema**

![gluejob4](/image/gluejob6.png)


7. Select **Create tables in your data target** and in **Data Store** select Amazon S3 in **Format** select **Parquet** and **Target Path** select S3 bucket where you want to store parquet file. 

Note - If there is no s3 bucket exists; please create new one. 

![gluejob4](/image/gluejob7.png)

8. Map the source columns to target columns and click on **Save Job and edit script**

Change the Column name, Data Type accordingly

```
    r_reason_sk               integer  ,
    r_reason_id               char(16) ,
    r_reason_desc             char(100)
```

![gluejob4](/image/gluejob8.png)


![gluejob10](/image/gluejob10.png)


Once job complete; you will able to see newly created files in your S3 bucket 

![gluejob10](/image/gluejob14.png)