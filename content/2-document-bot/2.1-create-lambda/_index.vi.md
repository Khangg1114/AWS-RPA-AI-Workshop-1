---
title : "Tạo Lambda Function"
date : "2025-07-21"
weight : 21
chapter : false
pre : " <b> 2.1 </b> "
---

#### Tổng quan
Tạo Lambda function đầu tiên sẽ tự động xử lý tài liệu được upload lên S3 bằng Amazon Textract.

#### Hướng dẫn từng bước

#### Bước 1: Điều hướng đến AWS Lambda
1. Trong **AWS Management Console**
2. Tìm kiếm **Lambda** trong thanh tìm kiếm
3. Click vào **AWS Lambda** từ kết quả
![Lambda-Setup-1](/images/2/LambdaFunction-Setup-1.png)

#### Bước 2: Tạo function mới
1. Click nút **Create function**
2. Bạn sẽ thấy trang "Create function" với các tùy chọn khác nhau
![Lambda-Setup-2](/images/2/LambdaFunction-Setup-2.png)

#### Bước 3: Chọn phương thức tạo function
**Function creation method:**
- Chọn **Author from scratch** (nên được chọn mặc định)
- Điều này cho phép chúng ta viết code riêng

#### Bước 4: Cấu hình thông tin cơ bản
**Function name:**
- Nhập: `document-processor`
- Tên này mô tả chức năng của function

**Runtime:**
- Chọn **Python 3.9** từ dropdown
- Python rất tốt cho tích hợp AWS và dịch vụ AI

**Architecture:**
- Giữ **x86_64** (mặc định)
- Đây là kiến trúc tiêu chuẩn
![Lambda-Setup-3](/images/2/LambdaFunction-Setup-3.png)

#### Bước 5: Cấu hình permissions
**Execution role:**
- Chọn **Use an existing role**
- Từ dropdown, chọn: `RPA-Lambda-Role`
- Đây là role chúng ta đã tạo ở bước trước

#### Bước 6: Cài đặt nâng cao (tùy chọn)
**Enable function URL:**
- Để **không tích** (chúng ta không cần web URL cho function này)

**Enable tags:**
- Bạn có thể thêm tags nếu muốn:
  - Key: `Project`, Value: `RPA-Workshop`
  - Key: `Function`, Value: `DocumentProcessor`

#### Bước 7: Tạo function
1. Xem lại cài đặt của bạn:
   - Function name: `document-processor`
   - Runtime: Python 3.9
   - Execution role: `RPA-Lambda-Role`
2. Click **Create function**
![Lambda-Setup-4](/images/2/LambdaFunction-Setup-4.png)
3. Đợi function được tạo (thường mất 10-20 giây)

#### Bước 8: Xác minh tạo function
1. Bạn nên thấy dashboard Lambda function
2. Ở trên cùng, bạn sẽ thấy: "Successfully created the function document-processor"
3. Trạng thái function nên hiển thị **Active**
![Lambda-Setup-5](/images/2/LambdaFunction-Setup-5.png)

#### Bước 9: Khám phá giao diện function
**Code tab:**
- Bạn sẽ thấy template code Python mặc định
- Chúng ta sẽ thay thế bằng code xử lý tài liệu ở bước tiếp theo

**Configuration tab:**
- Hiển thị cài đặt runtime, memory, timeout, v.v.
- Timeout mặc định là 3 giây (chúng ta sẽ tăng sau)

**Monitoring tab:**
- Sẽ hiển thị metrics khi function bắt đầu chạy
- Hiện tại trống vì chúng ta chưa chạy

{{% notice tip %}}
Tên function `document-processor` sẽ xuất hiện trong CloudWatch logs - nhớ điều này để debug!
{{% /notice %}}