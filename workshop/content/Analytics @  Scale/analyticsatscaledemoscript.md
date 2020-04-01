---
title: "Analytics @ Scale - demo script"
chapter: true
---

```
/******************************
*          Athena           *
******************************/
-- Ad-Hoc/Exploratory
SELECT COUNT(*) FROM store_sales;

SELECT * FROM store_sales LIMIT 100;

SELECT DISTINCT cd_education_status FROM customer_demographics;

SELECT min(d_date_sk), max(d_date_sk) FROM date_dim WHERE d_year = 2001

-- 40 seconds
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

-- -- 24 minutes
-- SELECT
-- ss_item_sk,
-- avg(ss_quantity) agg1,
-- avg(ss_list_price) agg2,
-- avg(ss_coupon_amt) agg3,
-- avg(ss_sales_price) agg4
-- FROM store_sales ss

-- INNER JOIN date_dim dd
--   ON ss.ss_sold_date_sk = dd.d_date_sk

-- INNER JOIN customer_demographics cd
--   ON ss.ss_cdemo_sk = cd.cd_demo_sk

-- WHERE dd.d_year = 2001
--   AND cd.cd_education_status = '4 yr Degree'

-- GROUP BY ss_item_sk


-- Athena TPCDS Database - Runtime 27m22s
select /* TPC-DS query7.tpl 0.2 */  i_item_id,
        avg(ss_quantity) agg1,
        avg(ss_list_price) agg2,
        avg(ss_coupon_amt) agg3,
        avg(ss_sales_price) agg4
 from store_sales, customer_demographics, date_dim, item, promotion
 where ss_sold_date_sk = d_date_sk and
       ss_item_sk = i_item_sk and
       ss_cdemo_sk = cd_demo_sk and
       ss_promo_sk = p_promo_sk and
       cd_gender = 'M' and
       cd_marital_status = 'M' and
       cd_education_status = '4 yr Degree' and
       (p_channel_email = 'N' or p_channel_event = 'N') and
       d_year = 2001
 group by i_item_id
 order by i_item_id
 limit 100;

/******************************
*          Redshift           *
******************************/

-- Disable Caching
set enable_result_cache_for_session to off;

-- All in Redshift - 19.536s
select /* TPC-DS query7.tpl 0.2 */  i_item_id,
        avg(ss_quantity) agg1,
        avg(ss_list_price) agg2,
        avg(ss_coupon_amt) agg3,
        avg(ss_sales_price) agg4
 from tpcds.store_sales, tpcds.customer_demographics, tpcds.date_dim, tpcds.item, tpcds.promotion
 where ss_sold_date_sk = d_date_sk and
       ss_item_sk = i_item_sk and
       ss_cdemo_sk = cd_demo_sk and
       ss_promo_sk = p_promo_sk and
       cd_gender = 'M' and
       cd_marital_status = 'M' and
       cd_education_status = '4 yr Degree' and
       (p_channel_email = 'N' or p_channel_event = 'N') and
       d_year = 2001
 group by i_item_id
 order by i_item_id
 limit 100;

select count(*) from tpcds.store_sales -- 86400345634 - 1 hr 6 min to load
select count(*) from tpcds.customer_demographics -- 1920800
select count(*) from tpcds.date_dim -- 73049
select count(*) from tpcds.item -- 462000
select count(*) from tpcds.promotion -- 2300


 -- Large Fact Table (store_sales) in Spectrum, Dimensional Tables in Redshift - 25.332
 
select /* TPC-DS query7.tpl 0.2 */  i_item_id,
        avg(ss_quantity) agg1,
        avg(ss_list_price) agg2,
        avg(ss_coupon_amt) agg3,
        avg(ss_sales_price) agg4 
 from spectrum_tpcds.store_sales, tpcds.customer_demographics, tpcds.date_dim, tpcds.item, tpcds.promotion
 where ss_sold_date_sk = d_date_sk and
       ss_item_sk = i_item_sk and
       ss_cdemo_sk = cd_demo_sk and
       ss_promo_sk = p_promo_sk and
       cd_gender = 'M' and 
       cd_marital_status = 'M' and
       cd_education_status = '4 yr Degree' and
       (p_channel_email = 'N' or p_channel_event = 'N') and
       d_year = 2001 
 group by i_item_id
 order by i_item_id
 limit 100; 

-- All In Spectrum - 26.123s
select /* TPC-DS query7.tpl 0.2 */  i_item_id, 
        avg(ss_quantity) agg1,
        avg(ss_list_price) agg2,
        avg(ss_coupon_amt) agg3,
        avg(ss_sales_price) agg4 
 from spectrum_tpcds.store_sales, spectrum_tpcds.customer_demographics, spectrum_tpcds.date_dim, spectrum_tpcds.item, spectrum_tpcds.promotion
 where ss_sold_date_sk = d_date_sk and
       ss_item_sk = i_item_sk and
       ss_cdemo_sk = cd_demo_sk and
       ss_promo_sk = p_promo_sk and
       cd_gender = 'M' and 
       cd_marital_status = 'M' and
       cd_education_status = '4 yr Degree' and
       (p_channel_email = 'N' or p_channel_event = 'N') and
       d_year = 2001 
 group by i_item_id
 order by i_item_id
 limit 100;

 -- All In Spectrum - Many Small Files - 27.100s
select /* TPC-DS query7.tpl 0.2 */  i_item_id,
        avg(ss_quantity) agg1,
        avg(ss_list_price) agg2,
        avg(ss_coupon_amt) agg3,
        avg(ss_sales_price) agg4 
 from spectrum_tpcds_many_small_files.store_sales, spectrum_tpcds_many_small_files.customer_demographics, spectrum_tpcds_many_small_files.date_dim, spectrum_tpcds_many_small_files.item, spectrum_tpcds_many_small_files.promotion
 where ss_sold_date_sk = d_date_sk and
       ss_item_sk = i_item_sk and
       ss_cdemo_sk = cd_demo_sk and
       ss_promo_sk = p_promo_sk and
       cd_gender = 'M' and 
       cd_marital_status = 'M' and
       cd_education_status = '4 yr Degree' and
       (p_channel_email = 'N' or p_channel_event = 'N') and
       d_year = 2001 
 group by i_item_id
 order by i_item_id
 limit 100;

-- Get Spectrum Metrics
SELECT
a.*,
(cast(a.sum_s3query_returned_bytes as float))/1024/1024/1024 as scandatainGB,
(cast(a.sum_s3_scanned_bytes as float))/1024/1024/1024/1024*5.00 as cost
FROM (
  SELECT 
  user,
  query,
  sum(s3_scanned_rows) as sum_s3_scanned_rows,
  sum(s3_scanned_bytes) as sum_s3_scanned_bytes,
  sum(s3query_returned_rows) as sum_s3query_returned_rows,
  sum(s3query_returned_bytes) as sum_s3query_returned_bytes

  FROM SVL_S3QUERY
  WHERE starttime::date = CURRENT_DATE
  GROUP BY user, query
) a ;
    


/******************************
*        Redshift ETL         *
******************************/

SELECT count(*) FROM tpcds.store_sales WHERE ss_sold_date_sk = 2452455
--26,074,154

-- 38 seconds
CREATE TABLE tpcds_stg.store_sales_2452455 AS
SELECT * FROM tpcds.store_sales WHERE ss_sold_date_sk = 2452455;

ALTER TABLE "store_sales_2452455" RENAME TO "store_sales_2452456";
UPDATE tpcds_stg.store_sales_2452456 SET ss_sold_date_sk = 2452456;

select * FROM store_sales_2452456 LIMIT 100;

select count(*) from tpcds.store_sales WHERE ss_sold_date_sk = 2452456;

-- 8 seconds
INSERT INTO tpcds.store_sales
SELECT * FROM tpcds_stg.store_sales_2452456;

-- Clear it back out
DELETE FROM tpcds.store_sales WHERE ss_sold_date_sk = 2452456;

-- Don't run this, already done
--UNLOAD ('SELECT * FROM tpcds_stg.store_sales_2452456') 
--TO 's3://jwyant-tpcds/stg/store_sales/store_sales_2452456_' iam_role 'arn:aws:iam::679645558400:role/mySpectrumRole' CSV GZIP;

-- Load from S3
DELETE tpcds_stg.store_sales_2452456;
COPY tpcds_stg.store_sales_2452456 FROM 's3://jwyant-tpcds/stg/store_sales/' 
IAM_ROLE 'arn:aws:iam::679645558400:role/mySpectrumRole' gzip delimiter ',' COMPUPDATE ON region 'us-east-1';
SELECT COUNT(*) FROM tpcds_stg.store_sales_2452456;

UPDATE tpcds_stg.store_sales_2452456 SET ss_list_price = ss_list_price*0.95;
DELETE FROM tpcds_stg.store_sales_2452456 WHERE MOD(ss_customer_sk,5) <> 0;

-- Start a new transaction
begin transaction;

-- ~45seconds
delete from tpcds.store_sales
using tpcds_stg.store_sales_2452456
where tpcds.store_sales.ss_sold_date_sk = tpcds_stg.store_sales_2452456.ss_sold_date_sk
  and tpcds.store_sales.ss_sold_time_sk = tpcds_stg.store_sales_2452456.ss_sold_time_sk
  and tpcds.store_sales.ss_item_sk = tpcds_stg.store_sales_2452456.ss_item_sk
  and tpcds.store_sales.ss_customer_sk = tpcds_stg.store_sales_2452456.ss_customer_sk
  and tpcds.store_sales.ss_sold_date_sk = 2452456;

-- Insert all the rows from the staging table into the target table
INSERT INTO tpcds.store_sales
SELECT * FROM tpcds_stg.store_sales_2452456;

-- End transaction and commit
end transaction;

select count(*) from tpcds.store_sales WHERE ss_sold_date_sk = 2452456;

-- Clean up
DELETE FROM tpcds.store_sales WHERE ss_sold_date_sk = 2452456;
DROP TABLE tpcds_stg.store_sales_2452456;
```