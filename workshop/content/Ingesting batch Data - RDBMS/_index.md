---
title: "Ingesting Batch Data - RDBMS"
chapter: true
---

## Overview

Welcome! In this workshop we will get hands-on with moving data from RDBMS MySQL running on [Amazon RDS](https://aws.amazon.com/rds/) and make it available for analytics.

## Lab Break Down

- For the purpose of this lab we are running MySQL 8 database instance on Amazon RDS. It is hosting TPC-DS dataset. The database/schema *tpcds* and all the tables inside it will be ingested.
- The ingestion will be carried out using [AWS Data Migration Service (DMS)](https://aws.amazon.com/dms/) and data will be saved in [Amazon Simple Storage Service (S3)](https://aws.amazon.com/s3/) bucket.
- We will then use [AWS Glue](https://aws.amazon.com/glue/) to perform schema discovery and store the schema in a centralized data catalog within AWS Glue.
