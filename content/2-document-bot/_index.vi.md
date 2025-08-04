---
title : "Tạo Document Processing Bot"
date : "2025-07-21"
weight : 20
chapter : false
pre : " <b> 2. </b> "
---

#### Tổng quan
Xây dựng Lambda function tự động xử lý tài liệu được upload lên S3 bằng Amazon Textract để trích xuất text và dữ liệu.

#### Những gì bạn sẽ xây dựng
- Lambda function được kích hoạt bởi S3 uploads
- Tích hợp với Amazon Textract cho AI document processing
- Trích xuất dữ liệu tự động từ invoices và forms

#### Các bước thực hiện

1. [Tạo Lambda Function](2.1-create-lambda/)
2. [Thêm Code Xử lý](2.2-add-code/)
3. [Cấu hình S3 Trigger](2.3-configure-trigger/)
4. [Test Document Processing](2.4-test-function/)

#### Kết quả mong đợi
- Lambda function xử lý tài liệu tự động
- Textract trích xuất text và form data
- Kết quả được lưu trong DynamoDB
- Hệ thống sẵn sàng cho email automation

{{% notice tip %}}
Thử upload các loại tài liệu khác nhau để xem Textract xử lý các định dạng khác nhau như thế nào!
{{% /notice %}}