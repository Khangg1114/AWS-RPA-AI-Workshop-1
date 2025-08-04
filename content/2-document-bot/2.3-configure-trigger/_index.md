---
title : "Configure S3 Trigger"
date : "2025-07-21"
weight : 23
chapter : false
pre : " <b> 2.3 </b> "
---

#### Overview
Set up S3 to automatically trigger our Lambda function whenever a document is uploaded to the bucket.

#### Step-by-Step Instructions

#### Step 1: Add Trigger to Lambda Function
1. In your `document-processor` Lambda function
2. Scroll down to the **Function overview** section
3. Click **Add trigger** button
![Configure-S3-Trigger-1](/images/2/Configure-S3-Trigger-1.png)

#### Step 2: Select Trigger Source
1. In the trigger configuration page
2. Click the **Select a source** dropdown
3. Choose **S3** from the list
![Configure-S3-Trigger-2](/images/2/Configure-S3-Trigger-2.png)

#### Step 3: Configure S3 Trigger Settings
**Bucket:**
- Select your bucket: `rpa-documents-[your-name]`
- This is the bucket you created in Step 1

**Event type:**
- Select **All object create events**
- This triggers on any file upload (PUT, POST, COPY)

**Prefix:**
- Enter: `documents/`
- This means only files in the `documents/` folder will trigger the function
- Leave empty if you want all uploads to trigger


#### Step 4: Acknowledge Permissions
1. You'll see a checkbox: **I acknowledge that using the same S3 bucket for both input and output is not recommended**
2. Check this box (we're not using the same bucket for output)
3. This is just a warning about best practices

#### Step 5: Add the Trigger
1. Review your settings:
   - Source: S3
   - Bucket: `rpa-documents-[your-name]`
   - Event type: All object create events
   - Prefix: `documents/` 
2. Click **Add** button
![Configure-S3-Trigger-3](/images/2/Configure-S3-Trigger-3.png)

#### Step 6: Verify Trigger Creation
1. You should see a success message
2. In the Function overview diagram, you'll see:
   - S3 bucket connected to your Lambda function
   - The trigger is now active
![Configure-S3-Trigger-4](/images/2/Configure-S3-Trigger-4.png)

#### Step 7: Check S3 Bucket Configuration
1. Go to **S3** in the AWS console
2. Open your `rpa-documents-[your-name]` bucket
3. Click the **Properties** tab
4. Scroll down to **Event notifications**
5. You should see a new event notification configured
![Configure-S3-Trigger-5](/images/2/Configure-S3-Trigger-5.png)

#### Understanding S3 Event Triggers
**How it works:**
1. File uploaded to S3 bucket
2. S3 generates an event notification
3. Event automatically invokes Lambda function
4. Lambda receives event with bucket and file details

**Event types available:**
- **All object create events**: Any upload method
- **PUT**: Direct file uploads
- **POST**: Form-based uploads
- **COPY**: Files copied within S3

**Prefix and Suffix filtering:**
- **Prefix**: Only files in specific folders
- **Suffix**: Only files with specific extensions
- **Both**: Can be combined for precise filtering

#### Troubleshooting
**If trigger creation fails:**
- Check that Lambda function has proper permissions
- Verify S3 bucket exists and you have access
- Make sure bucket name is spelled correctly

**If you don't see the trigger in Function overview:**
- Refresh the Lambda console page
- Check that the trigger was actually created
- Look for error messages in the console

**If S3 event notification doesn't appear:**
- Go to S3 bucket Properties tab
- Check Event notifications section
- The notification should show Lambda function as destination

**Common permission issues:**
- Lambda execution role needs S3 permissions (already configured)
- S3 needs permission to invoke Lambda (automatically configured)

#### Testing the Trigger (Preview)
Once configured, the trigger works like this:
1. Upload a document to `s3://your-bucket/documents/`
2. S3 automatically calls your Lambda function
3. Lambda processes the document with Textract
4. Results are stored in DynamoDB
5. You can see logs in CloudWatch

#### What's Next?
Your S3 trigger is configured and ready! Next, we'll test the complete document processing workflow by uploading a real document and verifying it gets processed automatically.

{{% notice tip %}}
The `documents/` prefix helps organize your files and prevents accidental processing of non-document files!
{{% /notice %}}