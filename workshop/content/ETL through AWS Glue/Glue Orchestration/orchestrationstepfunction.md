---
title: "Orchestration of Glue jobs using Step Functions"
chapter: true
---


### Abstract

This document demonstrates orchestration of AWS Glue jobs in an Extract Transform Load (ETL) pipeline using AWS Step functions and AWS Lambda. Using this solution, the need to install and maintain the open source tools using Amazon EC2 instances for ETL orchestration can be avoided. Most of the ETL jobs have separate extraction and transformation Glue jobs in an ETL pipeline that must be executed synchronously one after the other. The result of each Glue job will be persisted in a persistent layer like AWS S3 to allow for modularity of an ETL pipeline. Modularity of an ETL pipeline has two main advantages: 

a) refactor the business logic for transformations without affecting the other part of the pipeline

b) in case of failure of a Glue job in the ETL pipeline, the Glue job can be re-run without affecting other Glue jobs

In this solution running ETL pipeline with State Machines in Step Functions as the orchestration tool and using AWS Lambda for submitting and checking the status of Glue jobs are discussed. The decision to move forward or halt the ETL pipeline at a step in the pipeline is based upon the AWS Lambda function that checks the status of the Glue jobs. In this document, an example ETL pipeline with code is shown in three different steps (Glue jobs) of an ETL process where the first step is to clean the data from the source, the second step is to filter the data and the third step is to join the result from the second step with a lookup table to add metadata to the results. The results of the three steps are stored in S3. 

### Details

The goal is to orchestrate the Glue jobs so that, up on completion of one Glue job the following Glue job is kicked off automatically. The orchestration process is explained with an example containing three Glue jobs with each Glue job performing part of the total transformation in the ETL pipeline. The background section of this document explains the three steps of the ETL process and what each step of the process does with the dataset. The solution section explains how three different steps in the form of Glue jobs are aligned synchronously with AWS Step Functions, with AWS Lambda functions for submitting and status tracking of the Glue jobs.


### Background 

