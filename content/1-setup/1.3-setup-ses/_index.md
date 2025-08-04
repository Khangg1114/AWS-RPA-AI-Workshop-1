---
title : "Setup Amazon SES"
date : "2025-07-21"
weight : 13
chapter : false
pre : " <b> 1.3 </b> "
---

#### Overview
Configure Amazon Simple Email Service (SES) to send automated email notifications from our RPA system.

#### Step-by-Step Instructions

#### Step 1: Navigate to Amazon SES
1. In the **AWS Management Console**
2. Search for **SES** in the search bar
3. Click on **Amazon Simple Email Service** from the results
![SES-Setup-1](/images/1/SES-Setup-1.png)

#### Step 2: Verify Your Email Address
1. In the SES console, click **Verified identities** in the left menu
2. Click the **Create identity** button
3. You'll see the "Create identity" configuration page
![SES-Setup-2](/images/1/SES-Setup-2.png)

#### Step 3: Configure Email Identity
**Identity type:**
- Select **Email address** (not domain)

**Email address:**
- Enter your personal email address
- Example: `your-email@gmail.com`
- This will be used to send and receive RPA notifications

**Default configuration set:**
- Leave empty (we don't need this for the workshop)

#### Step 4: Create the Identity
1. Click **Create identity** button
2. You'll see a success message
3. Your email will appear in the identities list with status **Unverified**
![SES-Setup-3](/images/1/SES-Setup-3.png)

#### Step 5: Verify Your Email
1. Check your email inbox (including spam folder)
2. Look for email from **Amazon Web Services**
3. Subject: "Amazon SES Address Verification Request in region..."
![SES-Setup-4](/images/1/SES-Setup-4.png)
4. Click the **verification link** in the email
5. You'll see a confirmation page in your browser
![SES-Setup-5](/images/1/SES-Setup-5.png)

#### Step 6: Confirm Verification
1. Go back to the SES console
2. Refresh the page
3. Your email status should now show **Verified** with a green checkmark
4. If still unverified, wait a few minutes and refresh again
![SES-Setup-6](/images/1/SES-Setup-6.png)


{{% notice tip %}}
Keep your verified email address handy - you'll need to update it in the Lambda function code later!
{{% /notice %}}