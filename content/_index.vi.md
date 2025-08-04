---
title : "Nền tảng RPA đơn giản với AI trên AWS"
date :  "2025-07-21" 
weight : 1 
chapter : false
---

# Xây dựng Nền tảng RPA đơn giản với AI trên AWS

#### Tổng quan
Trong workshop này, bạn sẽ xây dựng một nền tảng **Robotic Process Automation (RPA)** đơn giản được tăng cường với **khả năng AI** sử dụng các dịch vụ AWS.
Bạn sẽ tạo một hệ thống tự động hóa cơ bản có thể:
- Xử lý tài liệu tự động bằng AI
- Trích xuất dữ liệu từ hóa đơn và biểu mẫu
- Gửi email tự động
- Giám sát và ghi log tất cả hoạt động
- Mở rộng theo khối lượng công việc

![RPA Workflow](/images/1/RPA-Workshop-Workflow.png)

#### Những gì bạn sẽ xây dựng
Một nền tảng RPA đơn giản với các thành phần:
- **Document Processing Bot**: Sử dụng Amazon Textract để trích xuất dữ liệu từ PDF
- **Email Automation Bot**: Gửi phản hồi tự động bằng SES
- **Data Processing**: Lưu trữ và xử lý dữ liệu trong DynamoDB
- **Monitoring Dashboard**: Metrics và alarms của CloudWatch

#### Các dịch vụ AWS được sử dụng
- **AWS Lambda**: Các hàm serverless cho RPA bots
- **Amazon Textract**: Xử lý tài liệu AI
- **Amazon SES**: Tự động hóa email
- **Amazon DynamoDB**: Lưu trữ dữ liệu
- **Amazon S3**: Lưu trữ file
- **Amazon CloudWatch**: Giám sát và logging

{{% notice note%}}
Chi phí ước tính: $5-10. Nhớ dọn dẹp tài nguyên sau khi hoàn thành.
{{% /notice%}}

#### Yêu cầu tiên quyết
- Tài khoản AWS với quyền cơ bản
- Kiến thức cơ bản về Python
- Text editor hoặc IDE

#### Cấu trúc Workshop

1. [Thiết lập môi trường AWS](1-setup/)
2. [Tạo Document Processing Bot](2-document-bot/)
3. [Xây dựng Email Automation Bot](3-email-bot/)
4. [Dọn dẹp tài nguyên](4-cleanup/)

#### Mục tiêu học tập
Sau khi hoàn thành workshop này, bạn sẽ:
- Hiểu các khái niệm và triển khai RPA
- Biết cách tích hợp dịch vụ AI với tự động hóa
- Có thể xây dựng các giải pháp tự động hóa serverless
- Hiểu về giám sát và logging của AWS