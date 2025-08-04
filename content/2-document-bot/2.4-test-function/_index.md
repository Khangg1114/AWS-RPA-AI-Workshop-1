---
title : "Test Document Processing"
date : "2025-07-21"
weight : 24
chapter : false
pre : " <b> 2.4 </b> "
---

#### Overview
Test the complete document processing workflow by uploading a real document and verifying it gets processed automatically.

#### Step-by-Step Instructions

#### Step 1: Prepare a Test Document
**Create a simple PDF**
1. Open any word processor (Word, Google Docs, etc.)
2. Create a simple document with:
   ```
   INVOICE
   
   Invoice Number: INV-001
   Date: January 21, 2025
   Customer: John Smith
   Amount: $100.00
   
   Description: RPA Workshop Test
   ```
3. Save as PDF: `test-invoice.pdf`
![Test-Document-Processing-1](/images/2/Test-Document-Processing-1.png)

#### Step 2: Upload Document to S3
1. Go to **S3** in AWS Console
2. Open your bucket: `rpa-documents-[your-name]`
![Test-Document-Processing-2](/images/2/Test-Document-Processing-2.png)
3. Click on the `documents` folder
![Test-Document-Processing-3](/images/2/Test-Document-Processing-3.png)
4. Click **Upload**
![Test-Document-Processing-4](/images/2/Test-Document-Processing-4.png)
5. Click **Add files** and select your test document
6. Click **Upload** to start the process
![Test-Document-Processing-5](/images/2/Test-Document-Processing-5.png)

#### Step 3: Monitor Lambda Execution
1. Go to **AWS Lambda** console
2. Click on your `document-processor` function
3. Click the **Monitor** tab
4. You should see:
   - Recent invocations increase by 1
   - Duration of the execution
   - No errors (hopefully!)

#### Step 4: Check CloudWatch Logs
1. In the Monitor tab, click **View CloudWatch logs**
2. Click on the most recent log stream
3. Look for log entries like:
   ```
   Processing document: test-invoice.pdf from bucket: rpa-documents-yourname
   Successfully processed document: test-invoice.pdf
   Document ID: [some-uuid]
   ```
![Test-Document-Processing-6](/images/2/Test-Document-Processing-6.png)

#### Step 5: Verify Data in DynamoDB
1. Go to **DynamoDB** in AWS Console
2. Click **Tables** → `rpa-processed-data`
3. Click **Explore table items**
4. You should see a new item with:
   - `document_id`: Unique identifier
   - `file_name`: Your uploaded file name
   - `extracted_text`: Text extracted from document
   - `key_value_pairs`: Form fields found
   - `processed_at`: Timestamp
   - `status`: "completed"
![Test-Document-Processing-7](/images/2/Test-Document-Processing-7.png)

#### Step 6: Review Extracted Data
Click on the DynamoDB item to see details:

**extracted_text:**
- Should contain the main text from your document
- May be truncated to 1000 characters

**key_value_pairs:**
- Should show form fields like:
  ```json
  {
    "Invoice Number": "INV-001",
    "Date": "January 21, 2025",
    "Customer": "John Smith",
    "Amount": "$100.00"
  }
  ```

**confidence_score:**
- Number between 0-100 indicating Textract's confidence
- Higher is better (usually 80+ for clear documents)
![Test-Document-Processing-8](/images/2/Test-Document-Processing-8.png)

#### Step 7: Test with Different Document Types
Try uploading different types of documents:

**Test 2: Image file**
- Take a photo of a receipt or form
- Upload as JPG/PNG to `documents/` folder
- Check if it processes correctly

**Test 3: Complex PDF**
- Upload a multi-page document
- Check processing time and results


#### What's Next?
Congratulations! Your document processing bot is working! The extracted data is now stored in DynamoDB and ready to be used by other parts of your RPA system.

Next, we'll create the email automation bot that will send you beautiful reports about the processed documents.

{{% notice tip %}}
Save some test documents - you'll need them to test the complete system later!
{{% /notice %}}