---
title : "Configure Email Schedule"
date : "2025-07-21"
weight : 33
chapter : false
pre : " <b> 3.3 </b> "
---

#### Overview
Set up automatic scheduling for your email automation bot using Amazon EventBridge (CloudWatch Events) to receive daily RPA reports.

#### Step-by-Step Instructions

#### Step 1: Add EventBridge Trigger
1. In your `email-automation-bot` Lambda function
2. Scroll down to the **Function overview** section
3. Click **Add trigger** button
![Configure-Email-Schedule-1](/images/3/Configure-Email-Schedule-1.png)

#### Step 2: Select EventBridge as Source
1. In the trigger configuration page
2. Click the **Select a source** dropdown
3. Choose **EventBridge (CloudWatch Events)** from the list
![Configure-Email-Schedule-2](/images/3/Configure-Email-Schedule-2.png)

#### Step 3: Configure EventBridge Rule
**Rule:**
- Select **Create a new rule**
- This will create a new scheduling rule specifically for your email bot

**Rule name:**
- Enter: `daily-rpa-email-schedule`
- This name describes what the rule does

**Rule description:**
- Enter: `Daily schedule for RPA email reports`
- Optional but helpful for documentation

#### Step 4: Set Schedule Expression
**Rule type:**
- Select **Schedule expression**
- This allows us to set up recurring schedules

**Schedule expression:**
- Enter: `rate(1 day)`
- This means the function will run once every day

**Alternative schedule options:**
- `rate(12 hours)` - Every 12 hours
- `rate(1 hour)` - Every hour (for testing)
- `cron(0 9 * * ? *)` - Every day at 9:00 AM UTC
- `cron(0 17 * * MON-FRI *)` - Weekdays at 5:00 PM UTC

#### Step 5: Configure Target
**Target:**
- Should automatically show your Lambda function
- If not, select **Lambda function** and choose `email-automation-bot`

**Configure input:**
- Select **Constant (JSON text)**
- Enter this JSON:
```json
{
  "source": "scheduled-event",
  "detail-type": "Daily RPA Report",
  "time": "daily"
}
```
![Configure-Email-Schedule-4](/images/3/Configure-Email-Schedule-4.png)

#### Step 6: Add the Trigger
1. Review your settings:
   - Source: EventBridge (CloudWatch Events)
   - Rule: daily-rpa-email-schedule
   - Schedule: rate(1 day)
   - Target: email-automation-bot
2. Click **Add** button
![Configure-Email-Schedule-3](/images/3/Configure-Email-Schedule-3.png)

#### Step 7: Verify Trigger Creation
1. You should see a success message
2. In the Function overview diagram, you'll see:
   - EventBridge connected to your Lambda function
   - The schedule trigger is now active
![Configure-Email-Schedule-5](/images/3/Configure-Email-Schedule-5.png)

#### Step 9: Check EventBridge Console
1. Go to **Amazon EventBridge** in AWS Console
2. Click **Rules** in the left menu
3. You should see your rule: `daily-rpa-email-schedule`
4. Click on it to see details and modify if needed

#### Understanding EventBridge Scheduling
**Schedule Expressions:**
- **Rate expressions**: `rate(value unit)`
  - Units: minute, minutes, hour, hours, day, days
  - Examples: `rate(5 minutes)`, `rate(1 hour)`, `rate(7 days)`

- **Cron expressions**: `cron(minute hour day month day-of-week year)`
  - More precise timing control
  - Examples: `cron(0 12 * * ? *)` (daily at noon UTC)

**Time Zones:**
- All schedules use UTC time
- Consider your local timezone when setting schedules
- 9 AM EST = 2 PM UTC (during standard time)

#### Schedule Recommendations
**For Production:**
- `rate(1 day)` - Daily summary (recommended)
- `cron(0 9 * * ? *)` - Daily at 9 AM UTC
- `cron(0 17 * * MON-FRI *)` - Weekdays at 5 PM UTC

**For Testing:**
- `rate(5 minutes)` - Every 5 minutes (delete after testing)
- `rate(1 hour)` - Hourly (for short-term testing)

#### Troubleshooting
**If trigger creation fails:**
- Check Lambda function permissions
- Verify EventBridge service is available in your region
- Try a different rule name if there's a conflict

**If emails don't send on schedule:**
- Check CloudWatch logs for the scheduled executions
- Verify the Lambda function runs without errors
- Check SES sending limits and quotas

**If you want to change the schedule:**
1. Go to EventBridge console
2. Find your rule: `daily-rpa-email-schedule`
3. Click **Edit** to modify the schedule expression

#### Managing the Schedule
**To temporarily disable:**
1. Go to EventBridge console
2. Find your rule and click **Disable**
3. Re-enable when ready

**To delete the schedule:**
1. In Lambda function, find the EventBridge trigger
2. Click the trigger and select **Delete**
3. Or delete from EventBridge console

#### Cost Considerations
**EventBridge pricing:**
- First 14 million events per month: Free
- Additional events: $1.00 per million events
- Daily emails = ~30 events per month (well within free tier)

**Lambda pricing:**
- Scheduled executions count toward your Lambda usage
- Daily execution = ~30 invocations per month
- Minimal cost impact

#### What's Next?
Your email automation is now fully automated! The system will:
1. Run daily at the scheduled time
2. Query processed documents from the last 24 hours
3. Generate and send beautiful HTML email reports
4. Continue running without manual intervention

Next, we'll test the complete system end-to-end to make sure everything works together perfectly.

{{% notice tip %}}
Start with daily reports, then adjust the frequency based on your document processing volume!
{{% /notice %}}