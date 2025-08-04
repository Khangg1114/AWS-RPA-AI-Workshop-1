---
title : "Test Email System"
date : "2025-07-21"
weight : 34
chapter : false
pre : " <b> 3.4 </b> "
---

#### Overview
Test the complete email automation system to ensure it generates and sends beautiful reports with your processed document data.

#### Step-by-Step Instructions

#### Step 1: Manual Function Test
1. In your `email-automation-bot` Lambda function
2. Click the **Test** button in the code editor
![Test-Email-System-1](/images/3/Test-Email-System-1.png)
3. If you haven't created a test event yet:
   - Select **Create new test event**
   - Event name: `manual-email-test`
   - Keep the default JSON template
4. Click **Test** to execute
![Test-Email-System-2](/images/3/Test-Email-System-2.png)

#### Step 2: Monitor Test Execution
1. Watch the **Execution result** section
2. Look for:
   - **Status**: Succeeded (green)
   - **Duration**: Should be under 10 seconds
   - **Logs**: Should show email sending process

#### Step 3: Check CloudWatch Logs
1. Click **Monitor** tab in your Lambda function
2. Click **View CloudWatch logs**
3. Click the most recent log stream
4. Look for log entries like:
   ```
   Email sent successfully. Message ID: [some-id]
   ```
![Test-Email-System-3](/images/3/Test-Email-System-3.png)
#### Step 4: Verify Email Delivery
1. Check your email inbox (the one you verified in SES)
2. Look for email with subject: "🤖 RPA Processing Summary - [date]"
3. If not in inbox, check spam/junk folder
4. Email should arrive within 1-2 minutes
![Test-Email-System-4](/images/3/Test-Email-System-4.png)
#### Step 5: Review Email Content
Open the email and verify it contains:

**Header Section:**
- Professional gradient header
- "RPA Processing Summary" title
- Generation timestamp

**Statistics Section:**
- Number of documents processed
- Average confidence score
- Total data points extracted

**Document Details:**
- Individual document entries
- File names and processing times
- Confidence scores
- Extracted key-value pairs (preview)

**Footer:**
- System status indicator
- Professional closing message

#### Step 6: Test with Recent Documents
1. Upload a new document to your S3 bucket (`documents/` folder)
2. Wait for it to be processed (check DynamoDB)
3. Run the email function test again
4. Verify the new document appears in the email

#### Step 7: Test Different Scenarios
**Test A: No recent documents**
1. Don't upload any documents for 24+ hours
2. Run email function
3. Should show "No Recent Documents" message
4. Should still include latest processed documents

**Test B: Multiple documents**
1. Upload 3-5 different documents
2. Wait for all to be processed
3. Run email function
4. Should show all documents with statistics

#### Step 8: Test Scheduled Execution
**Option A: Wait for scheduled time**
- If you set daily schedule, wait for next day
- Check email arrives automatically

**Option B: Create temporary hourly schedule**
1. Add another EventBridge trigger with `rate(1 hour)`
2. Wait 1 hour for automatic execution
3. Check email arrives without manual trigger
4. Delete the hourly trigger after testing


#### Troubleshooting
**If email doesn't arrive:**
- Check spam/junk folder first
- Verify email address is correct in code
- Check SES verified identities
- Look at CloudWatch logs for errors
- Check SES sending statistics

**If email content is wrong:**
- Verify DynamoDB has processed documents
- Check document processing timestamps
- Look for errors in email generation code
- Test with different document types

**If formatting is broken:**
- Check HTML syntax in email template
- Verify CSS styles are properly closed
- Test email in different email clients
- Check for special characters in document data

**Common error messages:**
- `"MessageRejected"`: Email address not verified
- `"SendingQuotaExceeded"`: SES daily limit reached
- `"Throttling"`: Sending too fast, retry later

#### Performance Optimization
**Email size considerations:**
- Current template shows max 10 documents
- Each document shows max 5 key-value pairs
- Total email size typically < 100KB
- Good balance between detail and performance

**Delivery optimization:**
- Emails typically deliver in 30-60 seconds
- SES has high deliverability rates
- Professional formatting improves inbox placement
- Avoid spam trigger words in content

#### Advanced Testing
**Load testing:**
1. Process 20+ documents
2. Run email function
3. Verify performance and formatting

**Error handling testing:**
1. Temporarily break DynamoDB permissions
2. Run email function
3. Should handle errors gracefully

**Schedule reliability:**
1. Monitor scheduled executions for several days
2. Verify consistent delivery times
3. Check for any missed executions

#### What's Next?
Congratulations! Your email automation system is working perfectly. You now have:
- Automatic document processing with AI
- Beautiful email reports with extracted data
- Scheduled daily summaries
- Professional monitoring and notifications



