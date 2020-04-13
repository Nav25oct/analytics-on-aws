---
title: "Crawling S3"
chapter: true
---

In this section, You are going to learn how you can use Glue crawler 

### Prerequisites

For this section please copy **100GB** of tpc-ds data to your S3 bucket from **s3://redshift-downloads/TPC-DS/100GB/**

```
e.g.

aws s3 cp --recursive s3://redshift-downloads/TPC-DS/100GB/ s3://analytics-on-aws-demo-100gb/100GB/
```

Crawl 100GB of txt data by using Glue Crawler

Go to AWS Glue --> Crawler --> Add Crawler

Provide Crawler Name

![analyticsatathean](/image/gluecrawl1.png)

Keep default name

![analyticsatathean](/image/athenatxt2.png)

Provide your S3 bucket name where you copied 100GB tpc-ds data 

```
e.g. 

s3://analytics-on-aws-demo-100gb/100GB/
```

Note - This is public bucket hence no extra access is needed 

![analyticsatathean](/image/gluecrawl3.png)

choose an existing role if you already have IAM Role created to crawl S3 bucket or else create an IAM role

![analyticsatathean](/image/gluecrawl4.png)

select Run on demand 

![analyticsatathean](/image/gluecrawl5.png)

Click on add databases and provide database name 

![analyticsatathean](/image/gluecrawl6.png)

Review and finish

![analyticsatathean](/image/gluecrawl7.png)

Run the crawler; it will take arrox 6+ minutes to crawl all the data

![analyticsatathean](/image/gluecrawlend.png)

After crawling you will able to see all the crawled data in tables section of Data Catalog

![analyticsatathean](/image/aftercrawl100gb.png)

You able to see Table Propert by clicking any of these tables

![tableproperty](/image/tableproperty100gb.png)

