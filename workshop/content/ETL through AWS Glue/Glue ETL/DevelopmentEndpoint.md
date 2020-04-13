---
title: "Development Endpoint"
chapter: true
---

You can use Glue Development Endpoint to iteratively develop and test your extract, transform, and load (ETL) scripts. You can create, edit, and delete development endpoints using the AWS Glue console or API. When you create a development endpoint, you provide configuration values to provision the development environment. These values tell AWS Glue how to set up the network so that you can access the endpoint securely and the endpoint can access your data stores.

You can then create a notebook that connects to the endpoint, and use your notebook to the author and test your ETL script. When you're satisfied with the results of your development process, you can create an ETL job that runs your script. With this process, you can add functions and debug your scripts in an interactive manner.

In this exercise, you will learn how to create Glue development endpoint and how to run or debug scripts there.

## Prerequisites

Create IAM Role for SageMaker

* Login to the AWS console, click the drop down on “Services” and type “IAM” in the search bar.

* Choose “Roles” in the IAM console and click on the tab “Create role” 

* "Select type of trusted entity" as "AWS Service" and "Choose the service that will use this role" as "SageMaker"

![sagemaker1](/image/sagemaker1.png)

In the “Select your use case” you will find “SageMaker” then click “Next: Permissions”

![sagemaker2](/image/sagemaker2.png)

Choose the policy “AmazonSageMakerFullAccess” and click “Next: Tags”

![sagemaker3](/image/sagemaker3.png)

Tags are optional, click “Next: Review” then enter “Role name” and then click “Create role”

![sagemaker5](/image/sagemaker5.png)

After the Role gets created search and select the role

![sagemaker6](/image/sagemaker6.png)

Click on the Role to open and click “Attach policies” in the “Permissions” tab

![sagemaker7](/image/sagemaker7.png)

Click and select the “AdministratorAccess” and click “Attach policy”

![sagemaker8](/image/sagemaker8.png)

You will find now 2 policies attached to the SageMaker role

![sagemaker9](/image/sagemaker9.png)


## Create a Glue Dev Endpoint

1. Go to Glue console and click on **Dev endpoints** from the left and then click on **Add endpoint**

![devend1](/image/devend1.png)

2. Provide a **Development endpoint name** and choose the Glue **IAM role** from the drop-down and click **Next**. This the same IAM role which was created in the above section. 

![sagemaker10](/image/sagemaker10.png)

3. For the “Networking” choose the VPC, private subnet which is created in the above section and the default security group and then click “Next”.

![sagemaker11](/image/sagemaker11.png)

4. Leave the ssh public key blank and click “Next” to review. After review click “Finish”.

![sagemaker12](/image/sagemaker12.png)

5. After clicking “Finish” you will find the “Provisioning status” move from “PROVISIONING” to “READY”. This step can take a few minutes.

![sagemaker13](/image/sagemaker13.png)

### Launch a SageMaker Notebook from Glue

1)	Click on “Notebooks” from the left-hand pane of the Glue console. Select “SageMaker notebooks” tab and click either “Create notebook” or “Create SageMaker notebook”.

![sagemaker14](/image/sagemaker14.png)

2. Configure the notebook by entering “Notebook name”, “Attach to development endpoint” by choosing the endpoint created earlier and also choose the IAM Role created earlier.

![sagemaker15](/image/sagemaker15.png)

3)	Also choose the VPC, Private Subnet and default security group and click “Create notebook”

![sagemaker16](/image/sagemaker16.png)

4. Click “Create notebook” at the bottom of the page

![sagemaker17](/image/sagemaker17.png)

5. The “Status” will change from “Starting” to “InService” when the Notebook is created.

![sagemaker18](/image/sagemaker18.png)

6. Click “Open Jupyter” to open the notebook in browser.

![sagemaker18](/image/sagemaker19.png)