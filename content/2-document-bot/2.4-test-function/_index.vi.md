---
title : "Test Document Processing"
date : "2025-07-21"
weight : 24
chapter : false
pre : " <b> 2.4 </b> "
---

#### Tổng quan
Test toàn bộ quy trình xử lý tài liệu bằng cách upload một tài liệu thực và xác minh nó được xử lý tự động.

#### Hướng dẫn từng bước

#### Bước 1: Chuẩn bị tài liệu test
**Tạo một PDF đơn giản**
1. Mở bất kỳ word processor nào (Word, Google Docs, v.v.)
2. Tạo một tài liệu đơn giản với:
   ```
   INVOICE
   
   Invoice Number: INV-001
   Date: January 21, 2025
   Customer: John Smith
   Amount: $100.00
   
   Description: RPA Workshop Test
   ```
3. Lưu dưới dạng PDF: `test-invoice.pdf`
![Test-Document-Processing-1](/images/2/Test-Document-Processing-1.png)

#### Bước 2: Upload tài liệu lên S3
1. Vào **S3** trong AWS Console
2. Mở bucket của bạn: `rpa-documents-[your-name]`
![Test-Document-Processing-2](/images/2/Test-Document-Processing-2.png)
3. Click vào thư mục `documents`
![Test-Document-Processing-3](/images/2/Test-Document-Processing-3.png)
4. Click **Upload**
![Test-Document-Processing-4](/images/2/Test-Document-Processing-4.png)
5. Click **Add files** và chọn tài liệu test của bạn
6. Click **Upload** để bắt đầu quá trình
![Test-Document-Processing-5](/images/2/Test-Document-Processing-5.png)

#### Bước 3: Giám sát Lambda execution
1. Vào **AWS Lambda** console
2. Click vào function `document-processor` của bạn
3. Click tab **Monitor**
4. Bạn nên thấy:
   - Recent invocations tăng lên 1
   - Duration của execution
   - Không có lỗi (hy vọng vậy!)

#### Bước 4: Kiểm tra CloudWatch logs
1. Trong tab Monitor, click **View CloudWatch logs**
2. Click vào log stream gần đây nhất
3. Tìm các log entries như:
   ```
   Processing document: test-invoice.pdf from bucket: rpa-documents-yourname
   Successfully processed document: test-invoice.pdf
   Document ID: [some-uuid]
   ```
![Test-Document-Processing-6](/images/2/Test-Document-Processing-6.png)

#### Bước 5: Xác minh dữ liệu trong DynamoDB
1. Vào **DynamoDB** trong AWS Console
2. Click **Tables** → `rpa-processed-data`
3. Click **Explore table items**
4. Bạn nên thấy một item mới với:
   - `document_id`: Định danh duy nhất
   - `file_name`: Tên file bạn đã upload
   - `extracted_text`: Text được trích xuất từ tài liệu
   - `key_value_pairs`: Các form fields được tìm thấy
   - `processed_at`: Timestamp
   - `status`: "completed"
![Test-Document-Processing-7](/images/2/Test-Document-Processing-7.png)

#### Bước 6: Xem lại dữ liệu đã trích xuất
Click vào DynamoDB item để xem chi tiết:

**extracted_text:**
- Nên chứa text chính từ tài liệu của bạn
- Có thể bị cắt ngắn ở 1000 ký tự

**key_value_pairs:**
- Nên hiển thị các form fields như:
  ```json
  {
    "Invoice Number": "INV-001",
    "Date": "January 21, 2025",
    "Customer": "John Smith",
    "Amount": "$100.00"
  }
  ```

**confidence_score:**
- Số giữa 0-100 chỉ độ tin cậy của Textract
- Càng cao càng tốt (thường 80+ cho tài liệu rõ ràng)
![Test-Document-Processing-8](/images/2/Test-Document-Processing-8.png)

#### Bước 7: Test với các loại tài liệu khác
Thử upload các loại tài liệu khác:

**Test 2: File hình ảnh**
- Chụp ảnh một hóa đơn hoặc form
- Upload dưới dạng JPG/PNG vào thư mục `documents/`
- Kiểm tra xem nó có xử lý đúng không

**Test 3: PDF phức tạp**
- Upload một tài liệu nhiều trang
- Kiểm tra thời gian xử lý và kết quả

