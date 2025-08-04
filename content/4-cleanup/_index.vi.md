---
title : "Dọn dẹp Tài nguyên"
date : "2025-07-21"
weight : 40
chapter : false
pre : " <b> 4. </b> "
---

#### Tổng quan
Dọn dẹp tất cả tài nguyên AWS được tạo trong workshop này để tránh phí phát sinh.

{{% notice warning %}}
**Quan trọng**: Điều này sẽ xóa vĩnh viễn tất cả tài nguyên và dữ liệu được tạo trong workshop. Hãy đảm bảo sao lưu bất kỳ dữ liệu quan trọng nào trước khi tiếp tục.
{{% /notice %}}

#### Bước 1: Xóa Lambda Functions
1. Vào **AWS Lambda** console
2. Chọn và xóa các functions này:
   - `document-processor`
   - `email-automation-bot`
   - Bất kỳ Lambda functions nào khác bạn đã tạo

#### Bước 2: Xóa S3 Bucket
1. Vào **S3** console
2. Chọn bucket của bạn `rpa-documents-[your-name]`
3. Click **Empty** để xóa tất cả objects
4. Xác nhận bằng cách gõ tên bucket
5. Click **Delete bucket**
6. Xác nhận xóa

#### Bước 3: Xóa DynamoDB Table
1. Vào **DynamoDB** console
2. Chọn table `rpa-processed-data`
3. Click **Delete**
4. Xác nhận bằng cách gõ `delete`

#### Bước 4: Xóa IAM Role
1. Vào **IAM** console
2. Click **Roles**
3. Tìm kiếm `RPA-Lambda-Role`
4. Chọn và click **Delete**
5. Xác nhận xóa

#### Bước 5: Xóa SES Verified Email
1. Vào **Amazon SES** console
2. Click **Verified identities**
3. Chọn địa chỉ email của bạn
4. Click **Delete**

#### Bước 6: Xóa CloudWatch Logs
1. Vào **CloudWatch** console
2. Click **Log groups**
3. Xóa log groups bắt đầu với `/aws/lambda/`
4. Chọn và xóa các log groups liên quan

#### Bước 7: Xóa API Gateway (Nếu đã tạo)
1. Vào **API Gateway** console
2. Chọn API của bạn (nếu bạn đã tạo)
3. Click **Actions** → **Delete**

#### Bước 8: Kiểm tra EventBridge Rules
1. Vào **EventBridge** console
2. Click **Rules**
3. Xóa bất kỳ rules nào bạn đã tạo (như `daily-rpa-summary`)

#### Xác minh cuối cùng
Kiểm tra các dịch vụ này để đảm bảo mọi thứ đã được xóa:
- ✅ Lambda functions đã xóa
- ✅ S3 bucket đã xóa
- ✅ DynamoDB table đã xóa
- ✅ IAM role đã xóa
- ✅ CloudWatch logs đã xóa

#### Kiểm tra chi phí
1. Vào **AWS Billing Dashboard**
2. Kiểm tra phần **Bills** cho phí tháng hiện tại
3. Hầu hết các dịch vụ sử dụng trong workshop này đều đủ điều kiện free tier
4. Bất kỳ phí nào cũng sẽ tối thiểu (dưới $5)

#### Những gì bạn đã học
Chúc mừng! Bạn đã thành công:
- ✅ Xây dựng một nền tảng RPA hoàn chỉnh với tích hợp AI
- ✅ Sử dụng nhiều dịch vụ AWS cùng nhau
- ✅ Triển khai tự động hóa xử lý tài liệu
- ✅ Tạo quy trình tự động hóa email
- ✅ Thiết lập giám sát và logging
- ✅ Học các thực hành tốt nhất của AWS cho việc dọn dẹp