---
title: "Analytics @ Scale - Athena"
chapter: true
weight: 16
---

## Analytics @ Scale - Athena

In this section of lab; we are going to learn importance of having right file type and partition in S3 for optimal athena query performance

## Contents

* [Athena Text data](#athena-text-data)
* [Athena Parquet data](#athena-parquet-data)

## Athena Text data 

In this section we are going to crawl 30 TB of text data using Glue Crawler and then run various query to analyze Athena performance.

Step 1 - Crawl 30TB of txt data by using Glue Crawler

Go to AWS Glue --> Crawler --> Add Crawler

Provide Crawler Name

![analyticsatathean](/image/athenatxt1.png)

Keep default name

![analyticsatathean](/image/athenatxt2.png)

Provide S3 bucket name 

```
s3://redshift-downloads/TPC-DS/30TB/
```

Note - This is public bucket hence no extra access is needed 

![analyticsatathean](/image/athenatxt3.png)

choose an existing role if you already have IAM Role created to crawl S3 bucket or else create an IAM role

![analyticsatathean](/image/athenatxt4.png)

select Run on demand 

![analyticsatathean](/image/athenatxt5.png)

Click on add databases and provide database name 

![analyticsatathean](/image/athenatxt6.png)

![analyticsatathean](/image/atheantxt7.png)

Review and finish

![analyticsatathean](/image/athenatxt8.png)

![analyticsatathean](/image/atheantxt9.png)

Go to Lake formation and select database and grant access super access to your newly created Glue IAM role

![analyticsatathean](/image/atheantxt10.png)


![analyticsatathean](/image/athena11.png)


![analyticsatathean](/image/athenatxt12.png)

Run the crawler; it will take arrox 6+ minutes to crawl all the data

![analyticsatathean](/image/athenatxt13.png)

Go to lake formation and grant select access to newly created tables to your aws IAM user.

Here in this example AWS IAM account user name is administrator

![analyticsatathean](/image/athenatxt14.png)

select table name; go to action --> Grant --> select respective IAM user name 

![analyticsatathean](/image/athenatxt15.png)


![analyticsatathean](/image/athenatxt16.png)

Go to athena and run below query and note Run time and data scanned 

```
SELECT COUNT(*) FROM store_sales;
```

In this example; Run time: 5 minutes 36 seconds, Data scanned: 2.22 TB

Record count - 86.4 billion

![analyticsatathean](/image/athenatxt17.png)


## Athena Parquet data 

Please follow steps describe in above section to crawl parquet data present in below s3 bucket

```
s3://jwyant-tpcds/processed/30tb/
````
Review the data structure in S3 bucket and multiple partitions created in it

![analyticsatathean](/image/athenaparquets3.png)

![analyticsatathean](/image/athenaparquets3_1.png)

![analyticsatathean](/image/athenaparquets3_2.png)

![analyticsatathean](/image/athenatxt18.png)


Review and finish and run the crawler; it will take approx 5+ minutes to crawl all the data

![analyticsatathean](/image/athenatxt19.png)

Go to athena and run below query and note Run time and data scanned 

```
SELECT count(*) FROM "tpcds"."store_sales"
```

In this example; Run time: 23.99 seconds, Data scanned: 0 KB

![analyticsatathean](/image/athenaparquet.png)



