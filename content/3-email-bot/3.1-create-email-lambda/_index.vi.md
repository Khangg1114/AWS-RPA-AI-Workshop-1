---
title : "Tạo Email Lambda Function"
date : "2025-07-21"
weight : 31
chapter : false
pre : " <b> 3.1 </b> "
---

#### Tổng quan
Tạo Lambda function thứ hai sẽ gửi thông báo email tự động với báo cáo tóm tắt từ hệ thống RPA của chúng ta.

#### Hướng dẫn Từng bước

#### Bước 1: Điều hướng đến AWS Lambda
1. Trong **AWS Management Console**
2. Vào **AWS Lambda** (bạn sẽ đã ở đây từ bước trước)
3. Bạn sẽ thấy function `document-processor` hiện có trong danh sách
![Email-Lambda-Function-1](/images/3/Email-Lambda-Function-1.png)

#### Bước 2: Tạo Function Khác
1. Click nút **Create function** lần nữa
2. Chúng ta đang tạo Lambda function thứ hai cho email automation

#### Bước 3: Cấu hình Thông tin Cơ bản
**Function creation method:**
- Chọn **Author from scratch**

**Function name:**
- Nhập: `email-automation-bot`
- Tên này rõ ràng cho biết nó dành cho email automation

**Runtime:**
- Chọn **Python 3.9** (giống function đầu tiên)

**Architecture:**
- Giữ **x86_64** (mặc định)
![Email-Lambda-Function-2](/images/3/Email-Lambda-Function-2.png)

#### Bước 4: Cấu hình Permissions
**Execution role:**
- Chọn **Use an existing role**
- Chọn: `RPA-Lambda-Role` (cùng role như trước)
- Role này đã có quyền SES và DynamoDB chúng ta cần

#### Bước 5: Cài đặt Tùy chọn
**Tags (khuyến nghị):**
- Key: `Project`, Value: `RPA-Workshop`
- Key: `Function`, Value: `EmailBot`
- Key: `Type`, Value: `Automation`

#### Bước 6: Tạo Function
1. Xem lại cài đặt của bạn:
   - Function name: `email-automation-bot`
   - Runtime: Python 3.9
   - Execution role: `RPA-Lambda-Role`
2. Click **Create function**
![Email-Lambda-Function-3](/images/3/Email-Lambda-Function-3.png)
3. Đợi tạo hoàn thành

#### Bước 7: Xác minh Function đã Tạo
1. Bạn sẽ thấy thông báo thành công
2. Trạng thái function sẽ là **Active**
3. Bây giờ bạn có 2 Lambda functions trong tài khoản

#### Bước 8: Cấu hình Function Settings
1. Click tab **Configuration**
2. Click **General configuration** → **Edit**
![Email-Lambda-Function-4](/images/3/Email-Lambda-Function-4.png)
3. Cập nhật các cài đặt này:
   - **Timeout**: `2 minutes` (120 giây)
   - **Memory**: `256 MB` (ít hơn document processor)
4. Click **Save**
![Email-Lambda-Function-5](/images/3/Email-Lambda-Function-5.png)

#### Bước 9: Xác minh Cả Hai Functions Tồn tại
1. Quay lại danh sách Lambda functions
2. Bạn sẽ thấy cả hai functions:
   - ✅ `document-processor` (để xử lý tài liệu)
   - ✅ `email-automation-bot` (để gửi emails)
![Email-Lambda-Function-6](/images/3/Email-Lambda-Function-6.png)

  

#### Hiểu về Email Bot Function
**Mục đích:**
- Gửi emails tóm tắt xử lý
- Tạo báo cáo HTML với dữ liệu đã trích xuất
- Cung cấp thông báo về trạng thái hệ thống RPA

**Tại sao function riêng biệt?**
- Cơ chế trigger khác nhau (scheduled vs. S3 events)
- Yêu cầu tài nguyên khác nhau (ít memory hơn)
- Dễ quản lý và debug riêng biệt
- Có thể schedule độc lập

**Yêu cầu Tài nguyên:**
- Ít memory hơn document processor (256 MB vs 512 MB)
- Timeout ngắn hơn (2 phút vs 5 phút)
- Chủ yếu làm database queries và gửi email

#### Khắc phục Sự cố
**Nếu tạo function thất bại:**
- Đảm bảo bạn dùng cùng IAM role: `RPA-Lambda-Role`
- Kiểm tra tên function không xung đột
- Xác minh bạn có quyền tạo Lambda

**Nếu không thấy cả hai functions:**
- Refresh trang Lambda console
- Đảm bảo bạn đang ở đúng AWS region
- Kiểm tra filters danh sách function

**Nếu cấu hình thất bại:**
- Thử set timeout và memory từng cái một
- Đảm bảo giá trị trong giới hạn AWS
- Refresh và thử lại nếu cần

#### Function Comparison
| Feature | document-processor | email-automation-bot |
|---------|-------------------|---------------------|
| Purpose | Process documents | Send email reports |
| Trigger | S3 upload events | Scheduled/manual |
| Memory | 512 MB | 256 MB |
| Timeout | 5 minutes | 2 minutes |
| Main Services | Textract, DynamoDB | SES, DynamoDB |

#### Bước tiếp theo là gì?
Email automation Lambda function của bạn đã sẵn sàng! Tiếp theo, chúng ta sẽ thêm Python code sẽ:
- Query tài liệu đã xử lý từ DynamoDB
- Tạo báo cáo email HTML đẹp
- Gửi thông báo qua Amazon SES

{{% notice tip %}}
Có các functions riêng biệt cho các tác vụ khác nhau là best practice trong kiến trúc serverless!
{{% /notice %}}