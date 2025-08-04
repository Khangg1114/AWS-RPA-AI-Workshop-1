---
title : "Test Hệ thống Email"
date : "2025-07-21"
weight : 34
chapter : false
pre : " <b> 3.4 </b> "
---

#### Tổng quan
Test toàn bộ hệ thống email automation để đảm bảo nó tạo và gửi báo cáo đẹp với dữ liệu tài liệu đã xử lý.

#### Hướng dẫn Từng bước

#### Bước 1: Manual Function Test
1. Trong Lambda function `email-automation-bot` của bạn
2. Click nút **Test** trong code editor
![Test-Email-System-1](/images/3/Test-Email-System-1.png)
3. Nếu bạn chưa tạo test event:
   - Chọn **Create new test event**
   - Event name: `manual-email-test`
   - Giữ JSON template mặc định
4. Click **Test** để thực thi
![Test-Email-System-2](/images/3/Test-Email-System-2.png)

#### Bước 2: Giám sát Test Execution
1. Xem phần **Execution result**
2. Tìm:
   - **Status**: Succeeded (màu xanh)
   - **Duration**: Sẽ dưới 10 giây
   - **Logs**: Sẽ hiển thị quá trình gửi email

#### Bước 3: Kiểm tra CloudWatch Logs
1. Click tab **Monitor** trong Lambda function
2. Click **View CloudWatch logs**
3. Click log stream gần nhất
4. Tìm log entries như:
   ```
   Email gửi thành công. Message ID: [some-id]
   ```
![Test-Email-System-3](/images/3/Test-Email-System-3.png)
#### Bước 4: Xác minh Email Delivery
1. Kiểm tra hộp thư email (cái bạn đã xác minh trong SES)
2. Tìm email với subject: "🤖 Báo cáo Xử lý RPA - [date]"
3. Nếu không có trong inbox, kiểm tra thư mục spam/junk
4. Email sẽ đến trong vòng 1-2 phút
![Test-Email-System-4](/images/3/Test-Email-System-4.png)

#### Bước 5: Xem lại Email Content
Mở email và xác minh nó chứa:

**Header Section:**
- Header gradient chuyên nghiệp
- Tiêu đề "Báo cáo Xử lý RPA"
- Timestamp tạo

**Statistics Section:**
- Số tài liệu đã xử lý
- Điểm confidence trung bình
- Tổng data points đã trích xuất

**Document Details:**
- Entries tài liệu riêng lẻ
- Tên file và thời gian xử lý
- Điểm confidence
- Key-value pairs đã trích xuất (preview)

**Footer:**
- Chỉ báo trạng thái hệ thống
- Thông báo đóng chuyên nghiệp

#### Bước 6: Test với Recent Documents
1. Upload tài liệu mới vào S3 bucket (folder `documents/`)
2. Đợi nó được xử lý (kiểm tra DynamoDB)
3. Chạy email function test lại
4. Xác minh tài liệu mới xuất hiện trong email

#### Bước 7: Test Các Scenarios Khác nhau
**Test A: Không có tài liệu gần đây**
1. Không upload tài liệu nào trong 24+ giờ
2. Chạy email function
3. Sẽ hiển thị thông báo "Không có Tài liệu Gần đây"
4. Vẫn sẽ bao gồm tài liệu đã xử lý mới nhất

**Test B: Nhiều tài liệu**
1. Upload 3-5 tài liệu khác nhau
2. Đợi tất cả được xử lý
3. Chạy email function
4. Sẽ hiển thị tất cả tài liệu với thống kê

#### Bước 8: Test Scheduled Execution
**Tùy chọn A: Đợi thời gian đã lên lịch**
- Nếu bạn set lịch hàng ngày, đợi ngày hôm sau
- Kiểm tra email đến tự động

**Tùy chọn B: Tạo lịch hàng giờ tạm thời**
1. Thêm EventBridge trigger khác với `rate(1 hour)`
2. Đợi 1 giờ cho automatic execution
3. Kiểm tra email đến mà không cần manual trigger
4. Xóa hourly trigger sau khi test


#### Khắc phục Sự cố
**Nếu email không đến:**
- Kiểm tra thư mục spam/junk trước
- Xác minh địa chỉ email đúng trong code
- Kiểm tra SES verified identities
- Xem CloudWatch logs để tìm lỗi
- Kiểm tra SES sending statistics

**Nếu nội dung email sai:**
- Xác minh DynamoDB có tài liệu đã xử lý
- Kiểm tra timestamps xử lý tài liệu
- Tìm lỗi trong email generation code
- Test với các loại tài liệu khác

**Nếu formatting bị hỏng:**
- Kiểm tra HTML syntax trong email template
- Xác minh CSS styles được đóng đúng
- Test email trong các email clients khác
- Kiểm tra ký tự đặc biệt trong document data

#### Tối ưu hóa Hiệu suất
**Cân nhắc kích thước email:**
- Template hiện tại hiển thị tối đa 10 tài liệu
- Mỗi tài liệu hiển thị tối đa 5 key-value pairs
- Tổng kích thước email thường < 100KB
- Cân bằng tốt giữa chi tiết và hiệu suất

**Tối ưu hóa delivery:**
- Emails thường deliver trong 30-60 giây
- SES có tỷ lệ deliverability cao
- Formatting chuyên nghiệp cải thiện inbox placement
- Tránh từ spam trigger trong nội dung

#### Advanced Testing
**Load testing:**
1. Xử lý 20+ tài liệu
2. Chạy email function
3. Xác minh hiệu suất và formatting

**Error handling testing:**
1. Tạm thời phá quyền DynamoDB
2. Chạy email function
3. Sẽ xử lý lỗi một cách graceful

**Schedule reliability:**
1. Giám sát scheduled executions vài ngày
2. Xác minh thời gian delivery nhất quán
3. Kiểm tra có executions bị thiếu không

#### Bước tiếp theo là gì?
Chúc mừng! Hệ thống email automation của bạn hoạt động hoàn hảo. Bây giờ bạn có:
- Xử lý tài liệu tự động với AI
- Báo cáo email đẹp với dữ liệu đã trích xuất
- Tóm tắt hàng ngày được lên lịch
- Giám sát và thông báo chuyên nghiệp

{{% notice tip %}}
Lưu screenshot của email report đẹp - nó tuyệt vời cho portfolio của bạn!
{{% /notice %}}