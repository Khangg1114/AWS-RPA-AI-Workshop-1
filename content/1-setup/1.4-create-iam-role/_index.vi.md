---
title : "Tạo IAM Role"
date : "2025-07-21"
weight : 14
chapter : false
pre : " <b> 1.4 </b> "
---

#### Tổng quan
Tạo IAM role cấp quyền cho Lambda functions truy cập các dịch vụ S3, DynamoDB, Textract, và SES.

#### Hướng dẫn từng bước

#### Bước 1: Điều hướng đến dịch vụ IAM
1. Trong **AWS Management Console**
2. Tìm kiếm **IAM** trong thanh tìm kiếm
3. Click vào **IAM** từ kết quả
![IAM-Setup-1](/images/1/IAMRole-Setup-1.png)

#### Bước 2: Tạo role mới
1. Trong IAM console, click **Roles** ở menu bên trái
2. Click nút **Create role**
3. Bạn sẽ thấy trang "Create role"
![IAM-Setup-2](/images/1/IAMRole-Setup-2.png)

#### Bước 3: Chọn trusted entity
**Trusted entity type:**
- Chọn **AWS service** (nên được chọn mặc định)

**Use case:**
- Chọn **Lambda** từ danh sách
- Điều này cho phép các Lambda functions assume role này
![IAM-Setup-3](/images/1/IAMRole-Setup-3.png)

#### Bước 4: Thêm permissions
1. Click **Next** để đến trang permissions
2. Bạn sẽ thấy hộp tìm kiếm để tìm policies
3. Chúng ta cần attach nhiều policies cho hệ thống RPA

**Tìm kiếm và chọn các policies này từng cái một:**

1. **AWSLambdaBasicExecutionRole**
   - Tìm kiếm: `AWSLambdaBasicExecutionRole`
   - Tích vào ô bên cạnh nó
   - Điều này cho phép Lambda ghi logs vào CloudWatch

2. **AmazonTextractFullAccess**
   - Tìm kiếm: `AmazonTextractFullAccess`
   - Tích vào ô bên cạnh nó
   - Điều này cho phép Lambda sử dụng Textract cho xử lý tài liệu

3. **AmazonSESFullAccess**
   - Tìm kiếm: `AmazonSESFullAccess`
   - Tích vào ô bên cạnh nó
   - Điều này cho phép Lambda gửi emails qua SES

4. **AmazonDynamoDBFullAccess**
   - Tìm kiếm: `AmazonDynamoDBFullAccess`
   - Tích vào ô bên cạnh nó
   - Điều này cho phép Lambda đọc/ghi DynamoDB tables

5. **AmazonS3FullAccess**
   - Tìm kiếm: `AmazonS3FullAccess`
   - Tích vào ô bên cạnh nó
   - Điều này cho phép Lambda đọc files từ S3
![IAM-Setup-4](/images/1/IAMRole-Setup-4.png)

#### Bước 5: Xem lại các policies đã chọn
Bạn nên có **5 policies** được chọn:
- ✅ AWSLambdaBasicExecutionRole
- ✅ AmazonTextractFullAccess
- ✅ AmazonSESFullAccess
- ✅ AmazonDynamoDBFullAccess
- ✅ AmazonS3FullAccess

Click **Next** để tiếp tục.
![IAM-Setup-5](/images/1/IAMRole-Setup-5.png)

#### Bước 6: Đặt tên và tạo role
**Tên role:**
- Nhập: `RPA-Lambda-Role`
- Tên này sẽ được sử dụng khi tạo Lambda functions

**Mô tả:**
- Nhập: `Role for RPA Lambda functions with access to S3, DynamoDB, Textract, and SES`

**Tags (tùy chọn):**
- Key: `Project`, Value: `RPA-Workshop`
- Key: `Purpose`, Value: `Lambda-Execution`
![IAM-Setup-6](/images/1/IAMRole-Setup-6.png)

#### Bước 7: Tạo role
1. Xem lại tất cả cài đặt:
   - Trusted entity: Lambda
   - Policies: 5 policies attached
   - Tên role: RPA-Lambda-Role
2. Click **Create role**
3. Bạn sẽ thấy thông báo thành công
![IAM-Setup-7](/images/1/IAMRole-Setup-7.png)

#### Bước 8: Xác minh tạo role
1. Bạn nên thấy `RPA-Lambda-Role` trong danh sách roles
![IAM-Setup-8](/images/1/IAMRole-Setup-8.png)
2. Click vào tên role để xem chi tiết
3. Kiểm tra tab **Permissions** - bạn nên thấy tất cả 5 policies
![IAM-Setup-9](/images/1/IAMRole-Setup-9.png)
4. Kiểm tra tab **Trust relationships** - nên hiển thị Lambda service
![IAM-Setup-10](/images/1/IAMRole-Setup-10.png)



{{% notice tip %}}
Tên role `RPA-Lambda-Role` sẽ được sử dụng khi tạo Lambda functions - nhớ chính xác tên này!
{{% /notice %}}