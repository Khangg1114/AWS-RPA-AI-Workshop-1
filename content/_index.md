---
title : "Simple RPA Platform with AI on AWS"
date :  "2025-07-21" 
weight : 1 
chapter : false
---

# Build a Simple RPA Platform with AI Integration on AWS

#### Overview
In this workshop, you will build a simple **Robotic Process Automation (RPA)** platform enhanced with **AI capabilities** using AWS services.
You'll create a basic automation system that can:
- Process documents automatically using AI
- Extract data from invoices and forms
- Send automated emails
- Monitor and log all activities
- Scale based on workload

![RPA Workflow](/images/1/RPA-Workshop-Workflow.png)

#### What You'll Build
A simple RPA platform with these components:
- **Document Processing Bot**: Uses Amazon Textract to extract data from PDFs
- **Email Automation Bot**: Sends automated responses using SES
- **Data Processing**: Stores and processes data in DynamoDB
- **Monitoring Dashboard**: CloudWatch metrics and alarms

#### AWS Services Used
- **AWS Lambda**: Serverless functions for RPA bots
- **Amazon Textract**: AI document processing
- **Amazon SES**: Email automation
- **Amazon DynamoDB**: Data storage
- **Amazon S3**: File storage
- **Amazon CloudWatch**: Monitoring and logging

{{% notice note%}}
Estimated cost: $5-10. Remember to clean up resources after completion.
{{% /notice%}}

#### Prerequisites
- AWS Account with basic permissions
- Basic knowledge of Python
- Text editor or IDE

#### Workshop Structure

1. [Setup AWS Environment](1-setup/)
2. [Create Document Processing Bot](2-document-bot/)
3. [Build Email Automation Bot](3-email-bot/)
4. [Cleanup Resources](4-cleanup/)

#### Learning Objectives
By the end of this workshop, you will:
- Understand RPA concepts and implementation
- Know how to integrate AI services with automation
- Be able to build serverless automation solutions
- Understand AWS monitoring and logging