---
title : "Add Email Automation Code"
date : "2025-07-21"
weight : 32
chapter : false
pre : " <b> 3.2 </b> "
---

#### Overview
Add Python code to the email Lambda function that will query processed documents and send beautiful HTML email reports.

#### Step-by-Step Instructions

#### Step 1: Open Email Function Code Editor
1. In your `email-automation-bot` Lambda function
![Add-Email-Automation-Code-1](/images/3/Add-Email-Automation-Code-1.png)
2. Make sure you're on the **Code** tab
3. You'll see the default Lambda code template

#### Step 2: Clear Default Code
1. Select all the default code in the editor
2. Delete it completely
3. The editor should be empty
![Add-Email-Automation-Code-2](/images/3/Add-Email-Automation-Code-2.png)

#### Step 3: Add Email Automation Code
Copy and paste this complete code:

```python
import json
import boto3
from datetime import datetime, timedelta

# Initialize AWS clients
ses = boto3.client('ses')
dynamodb = boto3.resource('dynamodb')

def lambda_handler(event, context):
    """
    Send email notifications for processed documents
    """
    try:
        # Get document processing results from DynamoDB
        table = dynamodb.Table('rpa-processed-data')
        
        # Get documents from the last 24 hours
        cutoff_time = (datetime.utcnow() - timedelta(hours=24)).isoformat()
        
        response = table.scan(
            FilterExpression='processed_at > :cutoff_time',
            ExpressionAttributeValues={':cutoff_time': cutoff_time}
        )
        
        recent_documents = response['Items']
        
        # If no recent documents, send a status email
        if not recent_documents:
            recent_documents = get_latest_documents(table, 5)  # Get last 5 documents
        
        # Generate email content
        email_content = generate_email_content(recent_documents)
        
        # Get recipient email from environment or use default
        recipient_email = "YOUR_EMAIL@example.com"  # Replace with your verified email
        
        # Send email using SES
        send_email(
            subject="🤖 RPA Processing Summary - " + datetime.utcnow().strftime('%Y-%m-%d'),
            body=email_content,
            recipient=recipient_email
        )
        
        return {
            'statusCode': 200,
            'body': json.dumps({
                'message': 'Email sent successfully',
                'documents_processed': len(recent_documents),
                'recipient': recipient_email
            })
        }
        
    except Exception as e:
        print(f"Error sending email: {str(e)}")
        return {
            'statusCode': 500,
            'body': json.dumps({
                'error': str(e),
                'message': 'Failed to send email'
            })
        }

def get_latest_documents(table, limit=5):
    """
    Get the most recent documents if no recent ones found
    """
    try:
        response = table.scan()
        items = response['Items']
        
        # Sort by processed_at timestamp (most recent first)
        sorted_items = sorted(items, key=lambda x: x.get('processed_at', ''), reverse=True)
        
        return sorted_items[:limit]
    except Exception as e:
        print(f"Error getting latest documents: {str(e)}")
        return []

def generate_email_content(documents):
    """
    Generate HTML email content from processed documents
    """
    html_content = f"""
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="UTF-8">
        <style>
            body {{ 
                font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
                line-height: 1.6; 
                color: #333; 
                max-width: 800px; 
                margin: 0 auto; 
                padding: 20px;
            }}
            .header {{ 
                background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
                color: white; 
                padding: 30px; 
                border-radius: 10px;
                text-align: center;
                margin-bottom: 30px;
            }}
            .header h1 {{ margin: 0; font-size: 28px; }}
            .header p {{ margin: 10px 0 0 0; opacity: 0.9; }}
            .summary {{ 
                background: #f8f9fa; 
                padding: 20px; 
                border-radius: 8px; 
                margin-bottom: 30px;
                border-left: 4px solid #667eea;
            }}
            .document {{ 
                background: white;
                border: 1px solid #e9ecef; 
                margin: 15px 0; 
                padding: 20px; 
                border-radius: 8px;
                box-shadow: 0 2px 4px rgba(0,0,0,0.1);
            }}
            .document h3 {{ 
                color: #495057; 
                margin-top: 0; 
                border-bottom: 2px solid #e9ecef;
                padding-bottom: 10px;
            }}
            .key-value {{ 
                margin: 8px 0; 
                padding: 5px 0;
            }}
            .key {{ 
                font-weight: 600; 
                color: #495057; 
                display: inline-block;
                min-width: 120px;
            }}
            .value {{ 
                color: #6c757d; 
                background: #f8f9fa;
                padding: 2px 8px;
                border-radius: 4px;
            }}
            .confidence {{ 
                background: #d4edda; 
                color: #155724; 
                padding: 4px 8px; 
                border-radius: 4px; 
                font-size: 12px;
                font-weight: bold;
            }}
            .footer {{ 
                margin-top: 40px; 
                padding: 20px; 
                background: #e9ecef; 
                border-radius: 8px; 
                text-align: center;
                color: #6c757d;
            }}
            .stats {{ 
                display: flex; 
                justify-content: space-around; 
                margin: 20px 0;
            }}
            .stat {{ 
                text-align: center; 
                padding: 15px;
            }}
            .stat-number {{ 
                font-size: 24px; 
                font-weight: bold; 
                color: #667eea;
            }}
            .stat-label {{ 
                font-size: 12px; 
                color: #6c757d; 
                text-transform: uppercase;
            }}
        </style>
    </head>
    <body>
        <div class="header">
            <h1>🤖 RPA Processing Summary</h1>
            <p>Generated on: {datetime.utcnow().strftime('%B %d, %Y at %H:%M UTC')}</p>
        </div>
        
        <div class="summary">
            <div class="stats">
                <div class="stat">
                    <div class="stat-number">{len(documents)}</div>
                    <div class="stat-label">Documents Processed</div>
                </div>
                <div class="stat">
                    <div class="stat-number">{calculate_avg_confidence(documents):.1f}%</div>
                    <div class="stat-label">Avg Confidence</div>
                </div>
                <div class="stat">
                    <div class="stat-number">{count_total_extractions(documents)}</div>
                    <div class="stat-label">Data Points Extracted</div>
                </div>
            </div>
        </div>
    """
    
    if not documents:
        html_content += """
        <div class="document">
            <h3>📭 No Recent Documents</h3>
            <p>No documents have been processed in the last 24 hours. Your RPA system is ready and waiting for new documents!</p>
        </div>
        """
    else:
        for i, doc in enumerate(documents[:10]):  # Show max 10 documents
            file_name = doc.get('file_name', 'Unknown File')
            processed_at = doc.get('processed_at', 'Unknown')
            confidence = doc.get('confidence_score', 0)
            
            # Format timestamp
            try:
                dt = datetime.fromisoformat(processed_at.replace('Z', '+00:00'))
                formatted_time = dt.strftime('%B %d, %Y at %H:%M UTC')
            except:
                formatted_time = processed_at
            
            html_content += f"""
            <div class="document">
                <h3>📄 {file_name}</h3>
                <div class="key-value">
                    <span class="key">Processed:</span> 
                    <span class="value">{formatted_time}</span>
                </div>
                <div class="key-value">
                    <span class="key">Confidence:</span> 
                    <span class="confidence">{confidence}%</span>
                </div>
            """
            
            # Add extracted key-value pairs
            key_values = doc.get('key_value_pairs', {})
            if key_values and isinstance(key_values, dict):
                html_content += "<h4>📊 Extracted Data:</h4>"
                for key, value in list(key_values.items())[:5]:  # Show first 5 key-value pairs
                    html_content += f"""
                    <div class="key-value">
                        <span class="key">{key}:</span> 
                        <span class="value">{value}</span>
                    </div>
                    """
            
            html_content += "</div>"
    
    html_content += f"""
        <div class="footer">
            <p><strong>✅ RPA System Status: Operational</strong></p>
            <p>Your automated document processing system is running smoothly.</p>
            <p><em>This is an automated message from your RPA platform on AWS.</em></p>
        </div>
    </body>
    </html>
    """
    
    return html_content

def calculate_avg_confidence(documents):
    """Calculate average confidence score"""
    if not documents:
        return 0
    
    confidences = [doc.get('confidence_score', 0) for doc in documents]
    valid_confidences = [c for c in confidences if c > 0]
    
    return sum(valid_confidences) / len(valid_confidences) if valid_confidences else 0

def count_total_extractions(documents):
    """Count total data points extracted"""
    total = 0
    for doc in documents:
        key_values = doc.get('key_value_pairs', {})
        if isinstance(key_values, dict):
            total += len(key_values)
    return total

def send_email(subject, body, recipient):
    """
    Send email using Amazon SES
    """
    try:
        response = ses.send_email(
            Source='YOUR_EMAIL@example.com',  # Replace with your verified email
            Destination={
                'ToAddresses': [recipient]
            },
            Message={
                'Subject': {
                    'Data': subject,
                    'Charset': 'UTF-8'
                },
                'Body': {
                    'Html': {
                        'Data': body,
                        'Charset': 'UTF-8'
                    }
                }
            }
        )
        
        print(f"Email sent successfully. Message ID: {response['MessageId']}")
        return response
        
    except Exception as e:
        print(f"Error sending email: {str(e)}")
        raise e
```

#### Step 4: Update Email Addresses
**IMPORTANT**: You must update the email addresses in the code:

1. Find this line: `recipient_email = "YOUR_EMAIL@example.com"`
2. Replace with your verified email from Step 1.3
3. Find this line: `Source='YOUR_EMAIL@example.com'`
4. Replace with the same verified email
5. Both should be the same email address you verified in SES

#### Step 5: Deploy the Code
1. After updating email addresses, click **Deploy**
2. Wait for the "Changes deployed" message
3. The code is now saved and ready to run
![Add-Email-Automation-Code-3](/images/3/Add-Email-Automation-Code-3.png)

#### Step 6: Test the Function
1. Click the **Test** button
2. Select **Create new test event**
3. Event name: `test-email`
4. Keep the default JSON (the function doesn't use event data)
5. Click **Test**
6. Check your email for the RPA summary report


#### Understanding the Email Code
**Main Features:**
- Queries DynamoDB for recent documents
- Generates professional HTML emails
- Includes statistics and summaries
- Handles cases with no recent documents
- Responsive email design

**Email Content:**
- Processing statistics
- Individual document details
- Extracted data preview
- Confidence scores
- Professional styling

#### Troubleshooting
**If email doesn't arrive:**
- Check spam/junk folder
- Verify email addresses are correct
- Check SES sending limits
- Look at CloudWatch logs for errors

**If function fails:**
- Check email addresses are verified in SES
- Verify DynamoDB table permissions
- Check for syntax errors in code

#### What's Next?
Your email automation code is ready! Next, we'll set up automatic scheduling so you receive daily reports without manual intervention.

{{% notice tip %}}
The email includes responsive design and works well on both desktop and mobile devices!
{{% /notice %}}