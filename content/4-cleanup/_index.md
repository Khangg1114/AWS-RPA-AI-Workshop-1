---
title : "Cleanup Resources"
date : "2025-07-21"
weight : 40
chapter : false
pre : " <b> 4. </b> "
---

#### Overview
Clean up all AWS resources created during this workshop to avoid ongoing charges.

{{% notice warning %}}
**Important**: This will permanently delete all resources and data created during the workshop. Make sure to backup any important data before proceeding.
{{% /notice %}}

#### Step 1: Delete Lambda Functions
1. Go to **AWS Lambda** console
2. Select and delete these functions:
   - `document-processor`
   - `email-automation-bot`
   - Any other Lambda functions you created

#### Step 2: Delete S3 Bucket
1. Go to **S3** console
2. Select your bucket `rpa-documents-[your-name]`
3. Click **Empty** to delete all objects
4. Confirm by typing the bucket name
5. Click **Delete bucket**
6. Confirm deletion

#### Step 3: Delete DynamoDB Table
1. Go to **DynamoDB** console
2. Select table `rpa-processed-data`
3. Click **Delete**
4. Confirm by typing `delete`

#### Step 4: Delete IAM Role
1. Go to **IAM** console
2. Click **Roles**
3. Search for `RPA-Lambda-Role`
4. Select and click **Delete**
5. Confirm deletion

#### Step 5: Remove SES Verified Email 
1. Go to **Amazon SES** console
2. Click **Verified identities**
3. Select your email address
4. Click **Delete**

#### Step 6: Delete CloudWatch Logs
1. Go to **CloudWatch** console
2. Click **Log groups**
3. Delete log groups starting with `/aws/lambda/`
4. Select and delete relevant log groups

#### Step 7: Delete API Gateway (If Created)
1. Go to **API Gateway** console
2. Select your API (if you created one)
3. Click **Actions** → **Delete**

#### Step 8: Check EventBridge Rules
1. Go to **EventBridge** console
2. Click **Rules**
3. Delete any rules you created (like `daily-rpa-summary`)

#### Final Verification
Check these services to ensure everything is deleted:
- ✅ Lambda functions deleted
- ✅ S3 bucket deleted
- ✅ DynamoDB table deleted
- ✅ IAM role deleted
- ✅ CloudWatch logs deleted

#### Cost Check
1. Go to **AWS Billing Dashboard**
2. Check **Bills** section for current month charges
3. Most services used in this workshop are free tier eligible
4. Any charges should be minimal (under $5)

#### What You've Learned
Congratulations! You've successfully:
- ✅ Built a complete RPA platform with AI integration
- ✅ Used multiple AWS services together
- ✅ Implemented document processing automation
- ✅ Created email automation workflows
- ✅ Set up monitoring and logging
- ✅ Learned AWS best practices for cleanup

