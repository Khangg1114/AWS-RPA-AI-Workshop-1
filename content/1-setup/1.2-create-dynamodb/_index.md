---
title : "Create DynamoDB Table"
date : "2025-07-21"
weight : 12
chapter : false
pre : " <b> 1.2 </b> "
---

#### Overview
Create an Amazon DynamoDB table to store the processed document data from our RPA system.

#### Step-by-Step Instructions

#### Step 1: Navigate to DynamoDB Service
1. In the **AWS Management Console**
2. Search for **DynamoDB** in the search bar
3. Click on **Amazon DynamoDB** from the results
![DynamoDB-Setup-1](/images/1/DynamoDB-Setup-1.png)

#### Step 2: Create New Table
1. Click the **Create table** button
2. You'll see the "Create table" configuration page
![DynamoDB-Setup-2](/images/1/DynamoDB-Setup-2.png)

#### Step 3: Configure Table Settings
**Table name:**
- Enter: `rpa-processed-data`
- This will store all our processed document information

**Partition key:**
- Key name: `document_id`
- Type: **String**
- This will be the unique identifier for each processed document
![DynamoDB-Setup-3](/images/1/DynamoDB-Setup-3.png)

#### Step 4: Table Settings
**Table settings:**
- Keep **Default settings** selected
- This will use on-demand billing (pay per use)

**Secondary indexes:**
- Leave empty (we don't need any for this workshop)

**Encryption at rest:**
- Keep **Owned by Amazon DynamoDB** (default)
- This provides encryption without extra cost

#### Step 5: Create the Table
1. Scroll down and click **Create table**
![DynamoDB-Setup-4](/images/1/DynamoDB-Setup-4.png)
2. You'll see "Creating table..." status
3. Wait for the table status to change to **Active** (usually takes 1-2 minutes)

#### Step 6: Verify Table Creation
1. You should see your table `rpa-processed-data` in the tables list
2. Click on the table name to view details
3. Check the **General information** tab:
   - Status should be **Active**
   - Partition key should be `document_id (S)`
![DynamoDB-Setup-5](/images/1/DynamoDB-Setup-5.png)

#### Step 7: Explore Table Structure
1. Click on **Explore table items** tab
2. You'll see "No items to display" - this is normal for a new table
3. Note the **Actions** dropdown - we'll use this later for testing


{{% notice tip %}}
The table name `rpa-processed-data` will be used in our Lambda functions - remember this exact name!
{{% /notice %}}