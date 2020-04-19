---
title: "Prerequisites"
chapter: true
---

To Complete **ETL through AWS Glue** demo; please complete below prerequisite -

**Setting up IAM Permissions for AWS Glue**

To create an IAM role for AWS Glue

1. Sign in to the AWS Management Console and open the IAM console at https://console.aws.amazon.com/iam/.

2. In the left navigation pane, choose Roles.

3. Choose Create role.

4. For role type, choose AWS Service, find and choose Glue, and choose Next: Permissions.

![glueIAM](/image/glueIAM.png)

5. On the Attach permissions policy page, choose the policies that contain the required permissions; for example, the AWS managed policy **AWSGlueServiceRole** for general AWS Glue permissions and the AWS managed policy **AmazonS3FullAccess** for access to Amazon S3 resources. Then choose Next: Review.

6. For Role name, enter a name for your role; for example, AWSGlueServiceRoleDefault. Create the role with the name prefixed with the string AWSGlueServiceRole to allow the role to be passed from console users to the service. AWS Glue provided policies expect IAM service roles to begin with AWSGlueServiceRole. Otherwise, you must add a policy to allow your users the iam:PassRole permission for IAM roles to match your naming convention. Choose Create Role.

![glueIAM2](/image/glueIAM2.png)


Note - ***If you plan to access Amazon S3 sources and targets that are encrypted with SSE-KMS, attach a policy that allows AWS Glue crawlers, jobs, and development endpoints to decrypt the data.*** 

```
{  
   "Version":"2012-10-17",
   "Statement":[  
      {  
         "Effect":"Allow",
         "Action":[  
            "kms:Decrypt"
         ],
         "Resource":[  
            "arn:aws:kms:*:account-id-without-hyphens:key/key-id"
         ]
      }
   ]
}
```
