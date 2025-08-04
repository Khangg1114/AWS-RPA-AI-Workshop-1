---
title : "Cấu hình S3 Trigger"
date : "2025-07-21"
weight : 23
chapter : false
pre : " <b> 2.3 </b> "
---

#### Tổng quan
Thiết lập S3 để tự động kích hoạt Lambda function của chúng ta bất cứ khi nào có tài liệu được upload vào bucket.

#### Hướng dẫn từng bước

#### Bước 1: Thêm trigger vào Lambda function
1. Trong Lambda function `document-processor` của bạn
2. Cuộn xuống đến phần **Function overview**
3. Click nút **Add trigger**
![Configure-S3-Trigger-1](/images/2/Configure-S3-Trigger-1.png)

#### Bước 2: Chọn nguồn trigger
1. Trong trang cấu hình trigger
2. Click dropdown **Select a source**
3. Chọn **S3** từ danh sách
![Configure-S3-Trigger-2](/images/2/Configure-S3-Trigger-2.png)

#### Bước 3: Cấu hình cài đặt S3 trigger
**Bucket:**
- Chọn bucket của bạn: `rpa-documents-[your-name]`
- Đây là bucket bạn đã tạo ở Bước 1

**Event type:**
- Chọn **All object create events**
- Điều này kích hoạt trên bất kỳ file upload nào (PUT, POST, COPY)

**Prefix:**
- Nhập: `documents/`
- Điều này có nghĩa chỉ các files trong thư mục `documents/` sẽ kích hoạt function
- Để trống nếu bạn muốn tất cả uploads đều kích hoạt

#### Bước 4: Xác nhận permissions
1. Bạn sẽ thấy checkbox: **I acknowledge that using the same S3 bucket for both input and output is not recommended**
2. Tích vào ô này (chúng ta không dùng cùng bucket cho output)
3. Đây chỉ là cảnh báo về best practices

#### Bước 5: Thêm trigger
1. Xem lại cài đặt của bạn:
   - Source: S3
   - Bucket: `rpa-documents-[your-name]`
   - Event type: All object create events
   - Prefix: `documents/`
2. Click nút **Add**
![Configure-S3-Trigger-3](/images/2/Configure-S3-Trigger-3.png)

#### Bước 6: Xác minh tạo trigger
1. Bạn nên thấy thông báo thành công
2. Trong sơ đồ Function overview, bạn sẽ thấy:
   - S3 bucket kết nối với Lambda function của bạn
   - Trigger bây giờ đã active
![Configure-S3-Trigger-4](/images/2/Configure-S3-Trigger-4.png)

#### Bước 7: Kiểm tra cấu hình S3 bucket
1. Vào **S3** trong AWS console
2. Mở bucket `rpa-documents-[your-name]` của bạn
3. Click tab **Properties**
4. Cuộn xuống đến **Event notifications**
5. Bạn nên thấy một event notification mới được cấu hình
![Configure-S3-Trigger-5](/images/2/Configure-S3-Trigger-5.png)



{{% notice tip %}}
Prefix `documents/` giúp tổ chức files và ngăn xử lý nhầm các file không phải tài liệu!
{{% /notice %}}