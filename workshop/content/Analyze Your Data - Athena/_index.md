---
title: "Analyze Your Data - Athena"
chapter: true
weight: 15
---

## Athena exploration

Walkthrough of different Athena functionality using the tpcds dataset; This dataset provides both csv and Parquet versions of 30TB data

## Contents

* [Table Definitions](#table-definitions)
* [CTAS Example](#ctas-example)
* [Optimization Techniques](#Optimization-Techniques)

## Table Definitions

```
CREATE EXTERNAL TABLE customer_address
(
 ca_address_sk int  ,
  ca_address_id string ,
  ca_street_number string ,      
  ca_street_name string,   
  ca_street_type string ,     
  ca_suite_number string ,    
  ca_city string ,         
  ca_county string ,       
  ca_state string ,            
  ca_zip string ,             
  ca_country string ,      
  ca_gmt_offset numeric(5,2) ,  
  ca_location_type string     
) 
ROW FORMAT DELIMITED
  FIELDS TERMINATED BY '|'
  ESCAPED BY '\\'
  LINES TERMINATED BY '\n'
LOCATION
  's3://redshift-downloads/TPC-DS/30TB/customer_address/'
TBLPROPERTIES ("skip.header.line.count"="1");


CREATE EXTERNAL TABLE store_sales
(
  ss_sold_date_sk int ,            
  ss_sold_time_sk int ,     
  ss_item_sk int,      
  ss_customer_sk int ,           
  ss_cdemo_sk int ,              
  ss_hdemo_sk int ,         
  ss_addr_sk int ,               
  ss_store_sk int ,           
  ss_promo_sk int ,           
  ss_ticket_number int,        
  ss_quantity int ,           
  ss_wholesale_cost decimal(7,2) ,          
  ss_list_price decimal(7,2) ,              
  ss_sales_price decimal(7,2) ,
  ss_ext_discount_amt decimal(7,2) ,             
  ss_ext_sales_price decimal(7,2) ,              
  ss_ext_wholesale_cost decimal(7,2) ,           
  ss_ext_list_price decimal(7,2) ,               
  ss_ext_tax decimal(7,2) ,                 
  ss_coupon_amt decimal(7,2) , 
  ss_net_paid decimal(7,2) ,   
  ss_net_paid_inc_tax decimal(7,2) ,             
  ss_net_profit decimal(7,2)                     
) ROW FORMAT DELIMITED
  FIELDS TERMINATED BY '|'
  ESCAPED BY '\\'
  LINES TERMINATED BY '\n'
LOCATION
  's3://redshift-downloads/TPC-DS/30TB/store_sales/'
TBLPROPERTIES ("skip.header.line.count"="1");


CREATE EXTERNAL TABLE customer_demographics
(
  cd_demo_sk int  ,   
  cd_gender string ,          
  cd_marital_status string ,   
  cd_education_status string , 
  cd_purchase_estimate int ,   
  cd_credit_rating string ,   
  cd_dep_count int ,             
  cd_dep_employed_count int ,    
  cd_dep_college_count int       
)
ROW FORMAT DELIMITED
  FIELDS TERMINATED BY '|'
  ESCAPED BY '\\'
  LINES TERMINATED BY '\n'
LOCATION
  's3://redshift-downloads/TPC-DS/30TB/customer_demographics/'
TBLPROPERTIES ("skip.header.line.count"="1");


CREATE EXTERNAL TABLE item
(
  i_item_sk int,                     
  i_item_id string ,      
  i_rec_start_date date,             
  i_rec_end_date date,               
  i_item_desc string ,         
  i_current_price decimal(7,2),      
  i_wholesale_cost decimal(7,2),     
  i_brand_id int,                   
  i_brand string ,                 
  i_class_id int,                   
  i_class string ,                 
  i_category_id int,                
  i_category string ,              
  i_manufact_id int,                
  i_manufact string ,              
  i_size string ,                  
  i_formulation string ,           
  i_color string ,            
  i_units string,             
  i_container string,         
  i_manager_id int,            
  i_product_name string       
) 
ROW FORMAT DELIMITED
  FIELDS TERMINATED BY '|'
  ESCAPED BY '\\'
  LINES TERMINATED BY '\n'
LOCATION
  's3://redshift-downloads/TPC-DS/30TB/item/'
TBLPROPERTIES ("skip.header.line.count"="1");

--- 2.74 seconds
select ca_state, count(ca_state) as num_of_cust_bystate from customer_address
group by ca_state
order by num_of_cust_bystate desc
limit 100

-- 13 minutes
SELECT
ss_item_sk,
avg(ss_quantity) agg1,
avg(ss_list_price) agg2,
avg(ss_coupon_amt) agg3,
avg(ss_sales_price) agg4
FROM store_sales ss
INNER JOIN (SELECT cd_demo_sk FROM customer_demographics WHERE cd_education_status = '4 yr Degree') cd
  ON ss.ss_cdemo_sk = cd.cd_demo_sk
WHERE ss_sold_date_sk between '2451911' and '2452275'
GROUP BY ss_item_sk;

```

## CTAS Example

Re-partition by marketplace and year to allow for efficient queries. (This takes ~5 minutes to run)

By default, the results are stored in a bucket automatically created in your account for Athena output: aws-athena-query-results-<account_id>-<region>.

See Athena CTAS examples for how to specify a specific S3 location with the external_location parameter.

```
CREATE TABLE num_of_cust_by_state
WITH (
      format = 'Parquet',
      parquet_compression = 'SNAPPY')
AS select ca_state, count(ca_state) as num_of_cust_bystate from customer_address
group by ca_state
order by num_of_cust_bystate desc;
```

```
CREATE TABLE num_of_cust_by_state_partitioned
WITH (
      format = 'Parquet',
      parquet_compression = 'SNAPPY',
      partitioned_by = ARRAY['ca_country'],
      external_location = 's3://analytics-on-aws/athena_partitions/')  
AS select ca_city, ca_zip , ca_state , ca_country from customer_address

```

Show different query times and data scanned

```
select ca_state, count(ca_state) as num_of_cust_bystate from customer_address
group by ca_state
order by num_of_cust_bystate desc
limit 100
```

```
select ca_state, count(ca_state) as num_of_cust_bystate from num_of_cust_by_state_partitioned
group by ca_state
order by num_of_cust_bystate desc
limit 100
```

## Optimization Techniques

If you frequently query data based on an ID and expect a limited amount of data to be returned, you can sort the original dataset by that ID and write it out to a limited number of objects on S3. Athena will use the parquet metadata to determine if it should read the underlying data.

One option is to use CTAS to create a derivative dataset and sort on the specific fields. This can take a while to run thanks to the sorting and the execution plan.

```
CREATE TABLE item_sorted
WITH (
  format='PARQUET'
) AS
select 
  i_item_sk, i_item_id, i_rec_start_date , i_rec_end_date , i_item_desc  , i_current_price , i_wholesale_cost , i_brand_id , i_brand  , i_class_id , i_class  , i_category_id , i_category  , i_manufact_id , i_manufact  , i_size  , i_formulation  , i_color  , i_units , i_container , i_manager_id , i_product_name
from item
order by i_item_id asc
```

Note that this only outputs seven heavily-skewed files, but all rows for a specific i_item_id should be in one file.

```
SELECT "$path", i_item_id , count(i_item_id) FROM "tpcds_athena"."item_sorted" 
where i_item_id = 'AAAAAAAAOPKEDAAA'
group by 1, 2 order by 1;
-- Run time: 4.05 seconds, Data scanned: 856.44 KB
```
vs

```
CREATE TABLE item_unsorted
WITH (
  format='PARQUET'
) AS
select 
  i_item_sk, i_item_id, i_rec_start_date , i_rec_end_date , i_item_desc  , i_current_price , i_wholesale_cost , i_brand_id , i_brand  , i_class_id , i_class  , i_category_id , i_category  , i_manufact_id , i_manufact  , i_size  , i_formulation  , i_color  , i_units , i_container , i_manager_id , i_product_name
from item
```

```
SELECT "$path", i_item_id , count(i_item_id) FROM "tpcds_athena"."item_unsorted" 
where i_item_id = 'AAAAAAAAOPKEDAAA'
group by 1, 2 order by 1;
-- Run time: 2.92 seconds, Data scanned: 1.6 MB
```





