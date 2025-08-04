---
title : "Create IAM Role"
date : "2025-07-21"
weight : 14
chapter : false
pre : " <b> 1.4 </b> "
---

#### Overview
Create an IAM role that gives our Lambda functions permission to access S3, DynamoDB, Textract, and SES services.

#### Step-by-Step Instructions

#### Step 1: Navigate to IAM Service
1. In the **AWS Management Console**
2. Search for **IAM** in the search bar
3. Click on **IAM** from the results
![IAM-Setup-1](/images/1/IAMRole-Setup-1.png)

#### Step 2: Create New Role
1. In the IAM console, click **Roles** in the left menu
2. Click the **Create role** button
3. You'll see the "Create role" page
![IAM-Setup-2](/images/1/IAMRole-Setup-2.png)

#### Step 3: Select Trusted Entity
**Trusted entity type:**
- Select **AWS service** (should be selected by default)

**Use case:**
- Select **Lambda** from the list
- This allows Lambda functions to assume this role
![IAM-Setup-3](/images/1/IAMRole-Setup-3.png)

#### Step 4: Add Permissions
1. Click **Next** to go to permissions page
2. You'll see a search box to find policies
3. We need to attach multiple policies for our RPA system

**Search and select these policies one by one:**

1. **AWSLambdaBasicExecutionRole**
   - Search: `AWSLambdaBasicExecutionRole`
   - Check the box next to it
   - This allows Lambda to write logs to CloudWatch

2. **AmazonTextractFullAccess**
   - Search: `AmazonTextractFullAccess`
   - Check the box next to it
   - This allows Lambda to use Textract for document processing

3. **AmazonSESFullAccess**
   - Search: `AmazonSESFullAccess`
   - Check the box next to it
   - This allows Lambda to send emails via SES

4. **AmazonDynamoDBFullAccess**
   - Search: `AmazonDynamoDBFullAccess`
   - Check the box next to it
   - This allows Lambda to read/write DynamoDB tables

5. **AmazonS3FullAccess**
   - Search: `AmazonS3FullAccess`
   - Check the box next to it
   - This allows Lambda to read files from S3
![IAM-Setup-4](/images/1/IAMRole-Setup-4.png)

#### Step 5: Review Selected Policies
You should have **5 policies** selected:
- ✅ AWSLambdaBasicExecutionRole
- ✅ AmazonTextractFullAccess
- ✅ AmazonSESFullAccess
- ✅ AmazonDynamoDBFullAccess
- ✅ AmazonS3FullAccess

Click **Next** to continue.
![IAM-Setup-5](/images/1/IAMRole-Setup-5.png)

#### Step 6: Name and Create Role
**Role name:**
- Enter: `RPA-Lambda-Role`
- This name will be used when creating Lambda functions

**Description:**
- Enter: `Role for RPA Lambda functions with access to S3, DynamoDB, Textract, and SES`

**Tags (optional):**
- Key: `Project`, Value: `RPA-Workshop`
- Key: `Purpose`, Value: `Lambda-Execution`
![IAM-Setup-6](/images/1/IAMRole-Setup-6.png)

#### Step 7: Create the Role
1. Review all settings:
   - Trusted entity: Lambda
   - Policies: 5 policies attached
   - Role name: RPA-Lambda-Role
2. Click **Create role**
3. You'll see a success message
![IAM-Setup-7](/images/1/IAMRole-Setup-7.png)

#### Step 8: Verify Role Creation
1. You should see `RPA-Lambda-Role` in the roles list
![IAM-Setup-8](/images/1/IAMRole-Setup-8.png)
2. Click on the role name to view details
3. Check the **Permissions** tab - you should see all 5 policies
![IAM-Setup-9](/images/1/IAMRole-Setup-9.png)
4. Check the **Trust relationships** tab - should show Lambda service
![IAM-Setup-10](/images/1/IAMRole-Setup-10.png)


{{% notice tip %}}
The role name `RPA-Lambda-Role` will be used when creating Lambda functions - remember this exact name!
{{% /notice %}}