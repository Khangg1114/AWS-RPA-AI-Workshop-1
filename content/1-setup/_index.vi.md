---
title : "Thiết lập Môi trường AWS"
date : "2025-07-21"
weight : 10
chapter : false
pre : " <b> 1. </b> "
---

#### Tổng quan
Thiết lập môi trường AWS với các dịch vụ và quyền cần thiết cho nền tảng RPA.

#### Những gì bạn sẽ tạo
- S3 bucket để lưu trữ tài liệu
- DynamoDB table để lưu trữ dữ liệu
- IAM roles và policies
- Cấu hình email SES

#### Các bước thực hiện

1. [Tạo S3 Bucket](1.1-create-s3-bucket/)
2. [Tạo DynamoDB Table](1.2-create-dynamodb/)
3. [Thiết lập Amazon SES](1.3-setup-ses/)
4. [Tạo IAM Role](1.4-create-iam-role/)

#### Bước 5: Xác minh thiết lập
Kiểm tra bạn đã tạo:
- ✅ S3 bucket: `rpa-documents-[your-name]`
- ✅ DynamoDB table: `rpa-processed-data`
- ✅ Email đã verify trong SES
- ✅ IAM role: `RPA-Lambda-Role`

#### Kết quả mong đợi
Môi trường AWS của bạn đã sẵn sàng với:
- Lưu trữ cho tài liệu và dữ liệu
- Dịch vụ email đã cấu hình
- Quyền phù hợp cho Lambda functions

{{% notice tip %}}
Ghi lại tên S3 bucket và địa chỉ email đã verify - bạn sẽ cần chúng ở các bước tiếp theo!
{{% /notice %}}