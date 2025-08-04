---
title : "Tạo DynamoDB Table"
date : "2025-07-21"
weight : 12
chapter : false
pre : " <b> 1.2 </b> "
---

#### Tổng quan
Tạo Amazon DynamoDB table để lưu trữ dữ liệu tài liệu đã xử lý từ hệ thống RPA của chúng ta.

#### Hướng dẫn từng bước

#### Bước 1: Điều hướng đến dịch vụ DynamoDB
1. Trong **AWS Management Console**
2. Tìm kiếm **DynamoDB** trong thanh tìm kiếm
3. Click vào **Amazon DynamoDB** từ kết quả
![DynamoDB-Setup-1](/images/1/DynamoDB-Setup-1.png)

#### Bước 2: Tạo table mới
1. Click nút **Create table**
2. Bạn sẽ thấy trang cấu hình "Create table"
![DynamoDB-Setup-2](/images/1/DynamoDB-Setup-2.png)

#### Bước 3: Cấu hình cài đặt table
**Tên table:**
- Nhập: `rpa-processed-data`
- Đây sẽ lưu trữ tất cả thông tin tài liệu đã xử lý của chúng ta

**Partition key:**
- Tên key: `document_id`
- Loại: **String**
- Đây sẽ là mã nhận dạng duy nhất cho mỗi tài liệu đã xử lý
![DynamoDB-Setup-3](/images/1/DynamoDB-Setup-3.png)

#### Bước 4: Cài đặt table
**Cài đặt table:**
- Giữ **Default settings** được chọn
- Điều này sẽ sử dụng on-demand billing (trả theo sử dụng)

**Secondary indexes:**
- Để trống (chúng ta không cần cho workshop này)

**Encryption at rest:**
- Giữ **Owned by Amazon DynamoDB** (mặc định)
- Điều này cung cấp mã hóa mà không tốn thêm chi phí

#### Bước 5: Tạo table
1. Cuộn xuống và click **Create table**
![DynamoDB-Setup-4](/images/1/DynamoDB-Setup-4.png)
2. Bạn sẽ thấy trạng thái "Creating table..."
3. Chờ trạng thái table chuyển thành **Active** (thường mất 1-2 phút)

#### Bước 6: Xác minh tạo table
1. Bạn sẽ thấy table `rpa-processed-data` của mình trong danh sách tables
2. Click vào tên table để xem chi tiết
3. Kiểm tra tab **General information**:
   - Status nên là **Active**
   - Partition key nên là `document_id (S)`
![DynamoDB-Setup-5](/images/1/DynamoDB-Setup-5.png)

#### Bước 7: Khám phá cấu trúc table
1. Click vào tab **Explore table items**
2. Bạn sẽ thấy "No items to display" - điều này bình thường cho table mới
3. Lưu ý dropdown **Actions** - chúng ta sẽ dùng nó sau để test



{{% notice tip %}}
Tên table `rpa-processed-data` sẽ được sử dụng trong Lambda functions - nhớ chính xác tên này!
{{% /notice %}}