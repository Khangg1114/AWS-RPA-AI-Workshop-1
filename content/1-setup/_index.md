---
title : "Setup AWS Environment"
date : "2025-07-21"
weight : 10
chapter : false
pre : " <b> 1. </b> "
---

#### Overview
Set up your AWS environment with the necessary services and permissions for the RPA platform.

#### What You'll Create
- S3 bucket for document storage
- DynamoDB table for data storage
- IAM roles and policies
- SES email configuration

#### Implementation Steps

1. [Create S3 Bucket](1.1-create-s3-bucket/)
2. [Create DynamoDB Table](1.2-create-dynamodb/)
3. [Setup Amazon SES](1.3-setup-ses/)
4. [Create IAM Role](1.4-create-iam-role/)

#### Step 5: Verify Setup
Check that you have created:
- ✅ S3 bucket: `rpa-documents-[your-name]`
- ✅ DynamoDB table: `rpa-processed-data`
- ✅ Verified email in SES
- ✅ IAM role: `RPA-Lambda-Role`

#### Expected Result
Your AWS environment is ready with:
- Storage for documents and data
- Email service configured
- Proper permissions for Lambda functions

{{% notice tip %}}
Write down your S3 bucket name and verified email address - you'll need them in the next steps!
{{% /notice %}}