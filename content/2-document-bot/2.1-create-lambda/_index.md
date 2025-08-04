---
title : "Create Lambda Function"
date : "2025-07-21"
weight : 21
chapter : false
pre : " <b> 2.1 </b> "
---

#### Overview
Create the first Lambda function that will automatically process documents uploaded to S3 using Amazon Textract.

#### Step-by-Step Instructions

#### Step 1: Navigate to AWS Lambda
1. In the **AWS Management Console**
2. Search for **Lambda** in the search bar
3. Click on **AWS Lambda** from the results
![Lambda-Setup-1](/images/2/LambdaFunction-Setup-1.png)

#### Step 2: Create New Function
1. Click the **Create function** button
2. You'll see the "Create function" page with different options
![Lambda-Setup-2](/images/2/LambdaFunction-Setup-2.png)

#### Step 3: Choose Function Creation Method
**Function creation method:**
- Select **Author from scratch** (should be selected by default)
- This allows us to write our own code

#### Step 4: Configure Basic Information
**Function name:**
- Enter: `document-processor`
- This name describes what the function does

**Runtime:**
- Select **Python 3.9** from the dropdown
- Python is great for AWS integrations and AI services

**Architecture:**
- Keep **x86_64** (default)
- This is the standard architecture
![Lambda-Setup-3](/images/2/LambdaFunction-Setup-3.png)

#### Step 5: Configure Permissions
**Execution role:**
- Select **Use an existing role**
- From the dropdown, choose: `RPA-Lambda-Role`
- This is the role we created in the previous step

#### Step 6: Advanced Settings (Optional)
**Enable function URL:**
- Leave **unchecked** (we don't need a web URL for this function)

**Enable tags:**
- You can add tags if you want:
  - Key: `Project`, Value: `RPA-Workshop`
  - Key: `Function`, Value: `DocumentProcessor`

#### Step 7: Create the Function
1. Review your settings:
   - Function name: `document-processor`
   - Runtime: Python 3.9
   - Execution role: `RPA-Lambda-Role`
2. Click **Create function**
![Lambda-Setup-4](/images/2/LambdaFunction-Setup-4.png)
3. Wait for the function to be created (usually takes 10-20 seconds)

#### Step 8: Verify Function Creation
1. You should see the Lambda function dashboard
2. At the top, you'll see: "Successfully created the function document-processor"
3. The function status should show as **Active**
![Lambda-Setup-5](/images/2/LambdaFunction-Setup-5.png)

#### Step 9: Explore the Function Interface
**Code tab:**
- You'll see a default Python code template
- We'll replace this with our document processing code in the next step

**Configuration tab:**
- Shows runtime settings, memory, timeout, etc.
- Default timeout is 3 seconds (we'll increase this later)

**Monitoring tab:**
- Will show metrics once the function starts running
- Currently empty since we haven't run it yet


{{% notice tip %}}
The function name `document-processor` will appear in CloudWatch logs - remember this for debugging!
{{% /notice %}}