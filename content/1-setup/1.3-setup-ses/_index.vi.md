---
title : "Thiết lập Amazon SES"
date : "2025-07-21"
weight : 13
chapter : false
pre : " <b> 1.3 </b> "
---

#### Tổng quan
Cấu hình Amazon Simple Email Service (SES) để gửi thông báo email tự động từ hệ thống RPA của chúng ta.

#### Hướng dẫn từng bước

#### Bước 1: Điều hướng đến Amazon SES
1. Trong **AWS Management Console**
2. Tìm kiếm **SES** trong thanh tìm kiếm
3. Click vào **Amazon Simple Email Service** từ kết quả
![SES-Setup-1](/images/1/SES-Setup-1.png)

#### Bước 2: Xác minh địa chỉ email của bạn
1. Trong SES console, click **Verified identities** ở menu bên trái
2. Click nút **Create identity**
3. Bạn sẽ thấy trang cấu hình "Create identity"
![SES-Setup-2](/images/1/SES-Setup-2.png)

#### Bước 3: Cấu hình email identity
**Loại identity:**
- Chọn **Email address** (không phải domain)

**Địa chỉ email:**
- Nhập địa chỉ email cá nhân của bạn
- Ví dụ: `your-email@gmail.com`
- Đây sẽ được sử dụng để gửi và nhận thông báo RPA

**Default configuration set:**
- Để trống (chúng ta không cần cho workshop này)

#### Bước 4: Tạo identity
1. Click nút **Create identity**
2. Bạn sẽ thấy thông báo thành công
3. Email của bạn sẽ xuất hiện trong danh sách identities với trạng thái **Unverified**
![SES-Setup-3](/images/1/SES-Setup-3.png)

#### Bước 5: Xác minh email của bạn
1. Kiểm tra hộp thư email của bạn (bao gồm thư mục spam)
2. Tìm email từ **Amazon Web Services**
3. Tiêu đề: "Amazon SES Address Verification Request in region..."
![SES-Setup-4](/images/1/SES-Setup-4.png)
4. Click vào **liên kết xác minh** trong email
5. Bạn sẽ thấy trang xác nhận trong trình duyệt
![SES-Setup-5](/images/1/SES-Setup-5.png)

#### Bước 6: Xác nhận xác minh
1. Quay lại SES console
2. Refresh trang
3. Trạng thái email của bạn bây giờ nên hiển thị **Verified** với dấu tích xanh
4. Nếu vẫn chưa verified, chờ vài phút và refresh lại
![SES-Setup-6](/images/1/SES-Setup-6.png)

