---
title: "Crawling S3"
chapter: true
---

In this section, You are going to learn how you can use Glue crawler 

Crawl 30TB of txt data by using Glue Crawler

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

Run the crawler; it will take arrox 6+ minutes to crawl all the data

![analyticsatathean](/image/athenatxt13.png)

After crawling you will able to see all the crawled data in tables section of Data Catalog

![analyticsatathean](/image/aftercrawl.png)

You able to see Table Propert by clicking any of these tables

![tableproperty](/image/tableproperty.png)

