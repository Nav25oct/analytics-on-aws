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

IAM --> Roles --> Create Roles --> AWS Services (Select Glue) --> Attach permession Policies (check mark AWSGlueServiceRole) --> Create Role

![gluejob2](/image/glueSparkJobRole.png)


3. From the IAM role drop-down list (Refresh it; if it not showing) - select ***tpcds-text-to-parquet-role***. 

![gluejob3](/image/glueIamRole.png)

4. Since you are creating Spark ETL job, so leave the rest of the fields as default.

Note - If you able to see new Glue 2.0 Version from drop-down in Glue Version; select it. 

![gluejob4](/image/sparkglue20.png)

5. select table which you want to convert to parquete - Hence in this example we are selecting **Catalog_retumn**

![gluejob4](/image/gluejob5.png)


6. Select **Change Schema**

![gluejob4](/image/gluejob6.png)


7. Select **Create tables in your data target** and in **Data Store** select Amazon S3 in **Format** select **Parquet** and **Target Path** select S3 bucket where you want to store parquet file. 

Note - If there is no s3 bucket exists; please create new one. 

![gluejob4](/image/gluejob7.png)

8. Map the source columns to target columns.

![gluejob4](/image/gluejob8.png)

Change the Column name, Data Type

```
  cr_returned_date_sk               int4 ,  
  cr_returned_time_sk               int4 , 
  cr_item_sk                        int4 not null , 
  cr_refunded_customer_sk           int4 ,
  cr_refunded_cdemo_sk              int4 ,   
  cr_refunded_hdemo_sk              int4 ,   
  cr_refunded_addr_sk               int4 ,    
  cr_returning_customer_sk          int4 ,
  cr_returning_cdemo_sk             int4 ,   
  cr_returning_hdemo_sk             int4 ,  
  cr_returning_addr_sk              int4 ,   
  cr_call_center_sk                 int4 ,      
  cr_catalog_page_sk                int4 ,     
  cr_ship_mode_sk                   int4 ,        
  cr_warehouse_sk                   int4 ,        
  cr_reason_sk                      int4 ,           
  cr_order_number                   int8 not null,
  cr_return_quantity                int4 ,     
  cr_return_amount                  numeric(7,2) ,
  cr_return_tax                     numeric(7,2) ,   
  cr_return_amt_inc_tax             numeric(7,2) ,
  cr_fee                            numeric(7,2) ,         
  cr_return_ship_cost               numeric(7,2) , 
  cr_refunded_cash                  numeric(7,2) ,    
  cr_reversed_charge                numeric(7,2) ,  
  cr_store_credit                   numeric(7,2) ,
  cr_net_loss                       numeric(7,2) 

```

![gluejob9](/image/gluejob9.png)



![gluejob10](/image/gluejob10.png)


Once job complete; you will able to see newly created files in your S3 bucket 


![gluejob10](/image/gluejob14.png)