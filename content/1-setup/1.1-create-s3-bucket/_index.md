---
title : "Create S3 Bucket"
date : "2025-07-21"
weight : 11
chapter : false
pre : " <b> 1.1 </b> "
---

#### Overview
Create an Amazon S3 bucket to store documents that will be processed by our RPA system.

#### Step-by-Step Instructions

#### Step 1: Navigate to S3 Service
1. Open the **AWS Management Console**
![S3-Setup-1](/images/1/S3-Setup-1.png)
2. In the search bar, type **S3**
![S3-Setup-2](/images/1/S3-Setup-2.png)
3. Click on **Amazon S3** from the results

#### Step 2: Create New Bucket
1. Click the **Create bucket** button
![S3-Setup-3](/images/1/S3-Setup-3.png)
2. You'll see the "Create bucket" configuration page

#### Step 3: Configure Bucket Settings
**Bucket name:**
- Enter: `rpa-documents-[your-name]`
- Replace `[your-name]` with your actual name or initials
- Example: `rpa-documents-fcj`
- Note: Bucket names must be globally unique

**AWS Region:**
- Select **ap-southeast-1**
- This region has the best support for all services we'll use
![S3-Setup-4](/images/1/S3-Setup-4.png)

#### Step 4: Configure Options
**Object Ownership:**
- Keep the default: **ACLs disabled (recommended)**

**Block Public Access settings:**
- Keep all checkboxes **checked** (this is secure)
- We don't need public access for this workshop

**Bucket Versioning:**
- Keep **Disable** (default)

**Default encryption:**
- Keep **Amazon S3 managed keys (SSE-S3)** (default)

#### Step 5: Create the Bucket
1. Scroll down and click **Create bucket**
2. You should see a success message
3. Your bucket will appear in the S3 buckets list

#### Step 6: Verify Bucket Creation
1. Find your bucket in the list: `rpa-documents-[your-name`
2. Click on the bucket name to open it
3. You should see an empty bucket with tabs: Objects, Properties, Permissions, etc.

#### Step 7: Create Folder Structure 
1. Inside your bucket, click **Create folder**
![S3-Setup-5](/images/1/S3-Setup-5.png)
2. Folder name: `documents`
3. Click **Create folder**
![S3-Setup-6](/images/1/S3-Setup-6.png)
4. This will help organize uploaded files
![S3-Setup-7](/images/1/S3-Setup-7.png)


{{% notice tip %}}
Write down your exact bucket name - you'll need it in later steps!
{{% /notice %}}