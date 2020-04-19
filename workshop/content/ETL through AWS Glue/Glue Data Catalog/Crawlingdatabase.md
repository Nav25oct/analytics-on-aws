---
title: "Crawling Database (JDBC)"
chapter: true
---

Let's create a JDBC crawler using the connection you just created to extract the schema from the TPC database.

1. Click on the **Crawlers** option on the left and then click on the **Add crawler** button.

![gluecrawler1](/image/gluecrawler1.png)

2. Enter **tpc-crawler** as the Crawler name and click **Next**.

![gluecrawler1](/image/gluecrawler2.png)

3. Select **Data stores** as the **Crawler source type**

![gluecrawler1](/image/gluecrawler3.png)

4. In this section you select the crawler type [S3, JDBC & DynamoDB]. Select JDBC as a data store, mysql-connector as the connection. You can crawl an entire schema/database or you can call any specific table or table with a prefix.

![gluecrawler][/image/gluecrawler4.png]

Note - if you want to crawl just single table provide **databasename/tablename** include path and if you have to exclude any table provide table name in **exclude pattens**

5. You are going to crawl only one data store, so select No from the option and click Next

![gluecrawler5](/image/gluecrawler5.png)

6. Select IAM role which you have created in Prerequisite section from the dropdown list and proceed next

![gluecrawler6](/image/gluecrawler6.png)

7. You can schedule a crawler in several ways but in this exercise, you run it on demand. Select Run on demand and move next.

![gluecrawler7](/image/gluecrawler7.png)

8. In this step, you decide where to store the crawler's output. Click on **Add database** 

![gluecrawler7](/image/gluecrawler8.png)

provide database name 

![gluecrawler7](/image/gluecrawler9.png)

![gluecrawler7](/image/gluecrawler10.png)

9. Verify all crawler information on the screen and click Finish to create the crawler.

![gluecrawler11](/image/gluecrawler11.png)

10. When the crawler is newly created, it will ask you if you want to run it now. Click on Run it now link. Alternatively, you can select the crawler and run the crawler from the Action.

![gluecrawler12](/image/gluecrawler12.png)


![gluecrawler13](/image/gluecrawler13.png)


![gluecrawler14](/image/gluecrawler14.png)