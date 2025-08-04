---
title : "Tạo S3 Bucket"
date : "2025-07-21"
weight : 11
chapter : false
pre : " <b> 1.1 </b> "
---

#### Tổng quan
Tạo Amazon S3 bucket để lưu trữ tài liệu sẽ được xử lý bởi hệ thống RPA của chúng ta.

#### Hướng dẫn từng bước

#### Bước 1: Điều hướng đến dịch vụ S3
1. Mở **AWS Management Console**
![S3-Setup-1](/images/1/S3-Setup-1.png)
2. Trong thanh tìm kiếm, gõ **S3**
![S3-Setup-2](/images/1/S3-Setup-2.png)
3. Click vào **Amazon S3** từ kết quả

#### Bước 2: Tạo bucket mới
1. Click nút **Create bucket**
![S3-Setup-3](/images/1/S3-Setup-3.png)
2. Bạn sẽ thấy trang cấu hình "Create bucket"

#### Bước 3: Cấu hình cài đặt bucket
**Tên bucket:**
- Nhập: `rpa-documents-[your-name]`
- Thay `[your-name]` bằng tên thật hoặc chữ cái viết tắt của bạn
- Ví dụ: `rpa-documents-fcj`
- Lưu ý: Tên bucket phải duy nhất toàn cầu

**AWS Region:**
- Chọn **ap-southeast-1**
- Region này có hỗ trợ tốt nhất cho tất cả dịch vụ chúng ta sẽ sử dụng
![S3-Setup-4](/images/1/S3-Setup-4.png)

#### Bước 4: Cấu hình tùy chọn
**Object Ownership:**
- Giữ mặc định: **ACLs disabled (recommended)**

**Block Public Access settings:**
- Giữ tất cả checkbox được **tích** (điều này bảo mật)
- Chúng ta không cần public access cho workshop này

**Bucket Versioning:**
- Giữ **Disable** (mặc định)

**Default encryption:**
- Giữ **Amazon S3 managed keys (SSE-S3)** (mặc định)

#### Bước 5: Tạo bucket
1. Cuộn xuống và click **Create bucket**
2. Bạn sẽ thấy thông báo thành công
3. Bucket của bạn sẽ xuất hiện trong danh sách S3 buckets

#### Bước 6: Xác minh tạo bucket
1. Tìm bucket của bạn trong danh sách: `rpa-documents-[your-name]`
2. Click vào tên bucket để mở nó
3. Bạn sẽ thấy bucket trống với các tab: Objects, Properties, Permissions, v.v.

#### Bước 7: Tạo cấu trúc thư mục
1. Bên trong bucket của bạn, click **Create folder**
![S3-Setup-5](/images/1/S3-Setup-5.png)
2. Tên thư mục: `documents`
3. Click **Create folder**
![S3-Setup-6](/images/1/S3-Setup-6.png)
4. Điều này sẽ giúp sắp xếp các file được upload
![S3-Setup-7](/images/1/S3-Setup-7.png)



{{% notice tip %}}
Ghi lại tên bucket chính xác - bạn sẽ cần nó ở các bước sau!
{{% /notice %}}