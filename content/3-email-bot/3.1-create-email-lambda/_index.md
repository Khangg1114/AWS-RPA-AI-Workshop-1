---
title : "Create Email Lambda Function"
date : "2025-07-21"
weight : 31
chapter : false
pre : " <b> 3.1 </b> "
---

#### Overview
Create a second Lambda function that will send automated email notifications with processing summaries from our RPA system.

#### Step-by-Step Instructions

#### Step 1: Navigate to AWS Lambda
1. In the **AWS Management Console**
2. Go to **AWS Lambda** (you should already be there from the previous step)
3. You'll see your existing `document-processor` function in the list
![Email-Lambda-Function-1](/images/3/Email-Lambda-Function-1.png)

#### Step 2: Create Another Function
1. Click the **Create function** button again
2. We're creating a second Lambda function for email automation

#### Step 3: Configure Basic Information
**Function creation method:**
- Select **Author from scratch**

**Function name:**
- Enter: `email-automation-bot`
- This name clearly indicates it's for email automation

**Runtime:**
- Select **Python 3.9** (same as the first function)

**Architecture:**
- Keep **x86_64** (default)
![Email-Lambda-Function-2](/images/3/Email-Lambda-Function-2.png)

#### Step 4: Configure Permissions
**Execution role:**
- Select **Use an existing role**
- Choose: `RPA-Lambda-Role` (same role as before)
- This role already has SES and DynamoDB permissions we need

#### Step 5: Optional Settings
**Tags (recommended):**
- Key: `Project`, Value: `RPA-Workshop`
- Key: `Function`, Value: `EmailBot`
- Key: `Type`, Value: `Automation`

#### Step 6: Create the Function
1. Review your settings:
   - Function name: `email-automation-bot`
   - Runtime: Python 3.9
   - Execution role: `RPA-Lambda-Role`
2. Click **Create function**
![Email-Lambda-Function-3](/images/3/Email-Lambda-Function-3.png)
3. Wait for creation to complete

#### Step 7: Verify Function Creation
1. You should see the success message
2. The function status should be **Active**
3. You now have 2 Lambda functions in your account

#### Step 8: Configure Function Settings
1. Click on the **Configuration** tab
2. Click **General configuration** → **Edit**
![Email-Lambda-Function-4](/images/3/Email-Lambda-Function-4.png)
3. Update these settings:
   - **Timeout**: `2 minutes` (120 seconds)
   - **Memory**: `256 MB` (less than document processor)
4. Click **Save**
![Email-Lambda-Function-5](/images/3/Email-Lambda-Function-5.png)

#### Step 9: Verify Both Functions Exist
1. Go back to the Lambda functions list
2. You should see both functions:
   - ✅ `document-processor` (for processing documents)
   - ✅ `email-automation-bot` (for sending emails)
![Email-Lambda-Function-6](/images/3/Email-Lambda-Function-6.png)

#### Understanding the Email Bot Function
**Purpose:**
- Send processing summary emails
- Generate HTML reports with extracted data
- Provide notifications about RPA system status

**Why a separate function?**
- Different trigger mechanism (scheduled vs. S3 events)
- Different resource requirements (less memory needed)
- Easier to manage and debug separately
- Can be scheduled independently

**Resource Requirements:**
- Less memory than document processor (256 MB vs 512 MB)
- Shorter timeout (2 minutes vs 5 minutes)
- Mainly does database queries and email sending

#### Troubleshooting
**If function creation fails:**
- Make sure you're using the same IAM role: `RPA-Lambda-Role`
- Check that the function name doesn't conflict
- Verify you have Lambda creation permissions

**If you can't see both functions:**
- Refresh the Lambda console page
- Make sure you're in the correct AWS region
- Check the function list filters

**If configuration fails:**
- Try setting timeout and memory one at a time
- Make sure values are within AWS limits
- Refresh and try again if needed

#### Function Comparison
| Feature | document-processor | email-automation-bot |
|---------|-------------------|---------------------|
| Purpose | Process documents | Send email reports |
| Trigger | S3 upload events | Scheduled/manual |
| Memory | 512 MB | 256 MB |
| Timeout | 5 minutes | 2 minutes |
| Main Services | Textract, DynamoDB | SES, DynamoDB |

#### What's Next?
Your email automation Lambda function is ready! Next, we'll add the Python code that will:
- Query processed documents from DynamoDB
- Generate beautiful HTML email reports
- Send notifications via Amazon SES

{{% notice tip %}}
Having separate functions for different tasks is a best practice in serverless architecture!
{{% /notice %}}