---
title: "Glue ETL Script"
chapter: true
---


### Find customer count by state

In this section; we are going to learn how to bring how use glue dev endpoint to develope pyspark script.

Prerequisite - Complete Lab **Create Development Endpoint** before this lab.

Click on Open Notebook

![glueNotebook](/image/glueNotebook.png)


![glueNotebook1](/image/glueNotebook1.png)


Create new files and set **Sparkmagic(PySpark)** and start running below script one by one  

![glueNotebook2](/image/glueNotebook2.png)



```pyspark
## Importing Python library 

import sys
from awsglue.transforms import *
from awsglue.utils import getResolvedOptions
from pyspark.context import SparkContext
from awsglue.context import GlueContext
from pyspark.sql import SQLContext

from awsglue.job import Job

glueContext = GlueContext(SparkContext.getOrCreate())

sqlContext = SQLContext(sc)

from pyspark.sql import *
```

    Starting Spark application



<table>
<tr><th>ID</th><th>YARN Application ID</th><th>Kind</th><th>State</th><th>Spark UI</th><th>Driver log</th><th>Current session?</th></tr><tr><td>2</td><td>application_1586891596511_0003</td><td>pyspark</td><td>idle</td><td><a target="_blank" href="http://ip-172-31-26-176.ec2.internal:20888/proxy/application_1586891596511_0003/">Link</a></td><td><a target="_blank" href="http://ip-172-31-28-195.ec2.internal:8042/node/containerlogs/container_1586891596511_0003_01_000001/livy">Link</a></td><td>✔</td></tr></table>



    FloatProgress(value=0.0, bar_style='info', description='Progress:', layout=Layout(height='25px', width='50%'),…


    SparkSession available as 'spark'.



    FloatProgress(value=0.0, bar_style='info', description='Progress:', layout=Layout(height='25px', width='50%'),…



```pyspark
## Open text file from S3 location

## Replace bucket name here with your bucket name

customer_addressRDD = sc.textFile("s3://analytics-on-aws-demo-100gb/100GB/customer_address/")
```


    FloatProgress(value=0.0, bar_style='info', description='Progress:', layout=Layout(height='25px', width='50%'),…



```pyspark
## Total count 

customer_addressRDD.count()
```


```pyspark
## Preview first record

customer_addressRDD.first()
```


```pyspark
## Convert RDD into Dataframe

from pyspark.sql import Row

customer_addressDF = customer_addressRDD.map(lambda ca : Row( 
                                                             ca_address_sk = int(ca.split('|')[0]),
                                                             ca_address_id = ca.split('|')[1], \
                                                             ca_street_number = ca.split('|')[2], \
                                                             ca_street_name = ca.split('|')[3], \
                                                             ca_street_type = ca.split('|')[4], \
                                                             ca_suite_number = ca.split('|')[5], \
                                                             ca_city = ca.split('|')[6], \
                                                             ca_county = ca.split('|')[7], \
                                                             ca_state = ca.split('|')[8], \
                                                             ca_zip = ca.split('|')[9], \
                                                             ca_country = ca.split('|')[10], \
                                                             ca_gmt_offset = ca.split('|')[11], \
                                                             ca_location_type = ca.split('|')[12]
                                                            )
                                            ).toDF()
```


    FloatProgress(value=0.0, bar_style='info', description='Progress:', layout=Layout(height='25px', width='50%'),…



```pyspark
## check dataframe schema

customer_addressDF.printSchema()
```


    FloatProgress(value=0.0, bar_style='info', description='Progress:', layout=Layout(height='25px', width='50%'),…


    root
     |-- ca_address_id: string (nullable = true)
     |-- ca_address_sk: long (nullable = true)
     |-- ca_city: string (nullable = true)
     |-- ca_country: string (nullable = true)
     |-- ca_county: string (nullable = true)
     |-- ca_gmt_offset: string (nullable = true)
     |-- ca_location_type: string (nullable = true)
     |-- ca_state: string (nullable = true)
     |-- ca_street_name: string (nullable = true)
     |-- ca_street_number: string (nullable = true)
     |-- ca_street_type: string (nullable = true)
     |-- ca_suite_number: string (nullable = true)
     |-- ca_zip: string (nullable = true)


```pyspark
## Review dataframe 

customer_addressDF.show(5)
```


    FloatProgress(value=0.0, bar_style='info', description='Progress:', layout=Layout(height='25px', width='50%'),…


    +----------------+-------------+---------------+-------------+-----------------+-------------+----------------+--------+------------------+----------------+--------------+---------------+------+
    |   ca_address_id|ca_address_sk|        ca_city|   ca_country|        ca_county|ca_gmt_offset|ca_location_type|ca_state|    ca_street_name|ca_street_number|ca_street_type|ca_suite_number|ca_zip|
    +----------------+-------------+---------------+-------------+-----------------+-------------+----------------+--------+------------------+----------------+--------------+---------------+------+
    |AAAAAAAABAAAAAAA|            1|      Fairfield|United States|  Maricopa County|           -7|           condo|      AZ|          Jackson |              18|       Parkway|      Suite 280| 86192|
    |AAAAAAAACAAAAAAA|            2|       Fairview|United States|      Taos County|           -7|           condo|      NM|    Washington 6th|             362|            RD|       Suite 80| 85709|
    |AAAAAAAADAAAAAAA|            3|Pleasant Valley|United States|      York County|           -5|   single family|      PA|Dogwood Washington|             585|        Circle|        Suite Q| 12477|
    |AAAAAAAAEAAAAAAA|            4|      Oak Ridge|United States|Kit Carson County|           -7|           condo|      CO|            Smith |             111|            Wy|        Suite A| 88371|
    |AAAAAAAAFAAAAAAA|            5|       Glendale|United States|     Barry County|           -6|   single family|      MO|          College |              31|          Blvd|      Suite 180| 63951|
    +----------------+-------------+---------------+-------------+-----------------+-------------+----------------+--------+------------------+----------------+--------------+---------------+------+
    only showing top 5 rows


```pyspark
## Create TempTable from Data frame for SparkSQL

customer_addressDF.registerTempTable('customer_address')
```


    FloatProgress(value=0.0, bar_style='info', description='Progress:', layout=Layout(height='25px', width='50%'),…



```pyspark
## Review records

sqlContext.sql("select * from customer_address limit 10").show()
```


    FloatProgress(value=0.0, bar_style='info', description='Progress:', layout=Layout(height='25px', width='50%'),…


    +----------------+-------------+---------------+-------------+-----------------+-------------+----------------+--------+------------------+----------------+--------------+---------------+------+
    |   ca_address_id|ca_address_sk|        ca_city|   ca_country|        ca_county|ca_gmt_offset|ca_location_type|ca_state|    ca_street_name|ca_street_number|ca_street_type|ca_suite_number|ca_zip|
    +----------------+-------------+---------------+-------------+-----------------+-------------+----------------+--------+------------------+----------------+--------------+---------------+------+
    |AAAAAAAABAAAAAAA|            1|      Fairfield|United States|  Maricopa County|           -7|           condo|      AZ|          Jackson |              18|       Parkway|      Suite 280| 86192|
    |AAAAAAAACAAAAAAA|            2|       Fairview|United States|      Taos County|           -7|           condo|      NM|    Washington 6th|             362|            RD|       Suite 80| 85709|
    |AAAAAAAADAAAAAAA|            3|Pleasant Valley|United States|      York County|           -5|   single family|      PA|Dogwood Washington|             585|        Circle|        Suite Q| 12477|
    |AAAAAAAAEAAAAAAA|            4|      Oak Ridge|United States|Kit Carson County|           -7|           condo|      CO|            Smith |             111|            Wy|        Suite A| 88371|
    |AAAAAAAAFAAAAAAA|            5|       Glendale|United States|     Barry County|           -6|   single family|      MO|          College |              31|          Blvd|      Suite 180| 63951|
    |AAAAAAAAGAAAAAAA|            6|       Lakeview|United States|    Chelan County|           -8|   single family|      WA|    Williams Sixth|              59|       Parkway|      Suite 100| 98579|
    |AAAAAAAAHAAAAAAA|            7|     Farmington|United States|                 |             |                |        |          Hill 7th|                |          Road|        Suite U| 39145|
    |AAAAAAAAIAAAAAAA|            8|          Union|United States|   Bledsoe County|           -5|       apartment|      TN|          Lincoln |             875|           Ct.|        Suite Y| 38721|
    |AAAAAAAAJAAAAAAA|            9|       New Hope|United States|     Perry County|           -6|           condo|      AL|        1st Laurel|             819|           Ave|       Suite 70| 39431|
    |AAAAAAAAKAAAAAAA|           10|   Martinsville|United States|   Haines Borough|           -9|           condo|      AK|   Woodland Poplar|             851|            ST|        Suite Y| 90419|
    +----------------+-------------+---------------+-------------+-----------------+-------------+----------------+--------+------------------+----------------+--------------+---------------+------+


```pyspark
## Customer count by state 

customer_countby_state = sqlContext.sql("select ca_state, count(ca_state) as ca_state_count from customer_address group by ca_state \
                order by ca_state_count desc")
```


    FloatProgress(value=0.0, bar_style='info', description='Progress:', layout=Layout(height='25px', width='50%'),…



```pyspark
customer_countby_state.show()
```


    FloatProgress(value=0.0, bar_style='info', description='Progress:', layout=Layout(height='25px', width='50%'),…


    +--------+--------------+
    |ca_state|ca_state_count|
    +--------+--------------+
    |      TX|         79639|
    |      GA|         49289|
    |      VA|         42362|
    |      KY|         37132|
    |      MO|         34002|
    |      KS|         32755|
    |      IL|         31500|
    |      NC|         31331|
    |      IA|         30883|
    |        |         30124|
    |      TN|         29848|
    |      NE|         29140|
    |      IN|         28416|
    |      OH|         27450|
    |      MN|         26756|
    |      MS|         25754|
    |      MI|         25226|
    |      OK|         24065|
    |      AR|         23157|
    |      WI|         22088|
    +--------+--------------+
    only showing top 20 rows


```pyspark
sqlContext.setConf("spark.sql.parquet.compression.codec","gzip")

customer_countby_state.write.parquet('s3://analytics-on-aws/tpcds/customer_count_state/');
```


    FloatProgress(value=0.0, bar_style='info', description='Progress:', layout=Layout(height='25px', width='50%'),…



```pyspark

```


Once you done with building your script. Go to File tab of your Jupyter Notebook and download as **Pyspark(txt)**

![glueNotebook3](/image/glueNotebook3.png)

After downloading file - place file to your S3 bucket 

![notebookscripts3](/image/notebookscripts3.png)

### Creating Glue Job with your own script 

Go to AWS Glue --> Jobs --> Add Job 

Provide Job Name
IAM Role 
Type - Spark
Glue Version - Latest from dropdown ( select Glue 2.0 if available for you)

This Job runs

* An existing script that you provide

S3 path where the script is stored

* Provide the S3 location of your S3 

![sparkjob](/image/sparkjob.png)

