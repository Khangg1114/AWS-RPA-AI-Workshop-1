---
title : "Cấu hình Lịch Email"
date : "2025-07-21"
weight : 33
chapter : false
pre : " <b> 3.3 </b> "
---

#### Tổng quan
Thiết lập scheduling tự động cho email automation bot bằng Amazon EventBridge (CloudWatch Events) để nhận báo cáo RPA hàng ngày.

#### Hướng dẫn Từng bước

#### Bước 1: Thêm EventBridge Trigger
1. Trong Lambda function `email-automation-bot` của bạn
2. Cuộn xuống phần **Function overview**
3. Click nút **Add trigger**
![Configure-Email-Schedule-1](/images/3/Configure-Email-Schedule-1.png)

#### Bước 2: Chọn EventBridge làm Source
1. Trong trang cấu hình trigger
2. Click dropdown **Select a source**
3. Chọn **EventBridge (CloudWatch Events)** từ danh sách
![Configure-Email-Schedule-2](/images/3/Configure-Email-Schedule-2.png)

#### Bước 3: Cấu hình EventBridge Rule
**Rule:**
- Chọn **Create a new rule**
- Điều này sẽ tạo rule scheduling mới riêng cho email bot

**Rule name:**
- Nhập: `daily-rpa-email-schedule`
- Tên này mô tả chức năng của rule

**Rule description:**
- Nhập: `Lịch hàng ngày cho báo cáo email RPA`
- Tùy chọn nhưng hữu ích cho documentation

#### Bước 4: Set Schedule Expression
**Rule type:**
- Chọn **Schedule expression**
- Điều này cho phép chúng ta thiết lập lịch lặp lại

**Schedule expression:**
- Nhập: `rate(1 day)`
- Có nghĩa function sẽ chạy một lần mỗi ngày

**Tùy chọn lịch khác:**
- `rate(12 hours)` - Mỗi 12 giờ
- `rate(1 hour)` - Mỗi giờ (để testing)
- `cron(0 9 * * ? *)` - Mỗi ngày lúc 9:00 AM UTC
- `cron(0 17 * * MON-FRI *)` - Các ngày trong tuần lúc 5:00 PM UTC

#### Bước 5: Cấu hình Target
**Target:**
- Sẽ tự động hiển thị Lambda function của bạn
- Nếu không, chọn **Lambda function** và chọn `email-automation-bot`

**Configure input:**
- Chọn **Constant (JSON text)**
- Nhập JSON này:
```json
{
  "source": "scheduled-event",
  "detail-type": "Daily RPA Report",
  "time": "daily"
}
```
![Configure-Email-Schedule-4](/images/3/Configure-Email-Schedule-4.png)

#### Bước 6: Thêm Trigger
1. Xem lại cài đặt của bạn:
   - Source: EventBridge (CloudWatch Events)
   - Rule: daily-rpa-email-schedule
   - Schedule: rate(1 day)
   - Target: email-automation-bot
2. Click nút **Add**
![Configure-Email-Schedule-3](/images/3/Configure-Email-Schedule-3.png)

#### Bước 7: Xác minh Trigger đã Tạo
1. Bạn sẽ thấy thông báo thành công
2. Trong sơ đồ Function overview, bạn sẽ thấy:
   - EventBridge kết nối với Lambda function
   - Schedule trigger hiện đã active
![Configure-Email-Schedule-5](/images/3/Configure-Email-Schedule-5.png)

#### Bước 9: Kiểm tra EventBridge Console
1. Vào **Amazon EventBridge** trong AWS Console
2. Click **Rules** ở menu bên trái
3. Bạn sẽ thấy rule: `daily-rpa-email-schedule`
4. Click vào để xem chi tiết và chỉnh sửa nếu cần

  

#### Hiểu về EventBridge Scheduling
**Schedule Expressions:**
- **Rate expressions**: `rate(value unit)`
  - Units: minute, minutes, hour, hours, day, days
  - Ví dụ: `rate(5 minutes)`, `rate(1 hour)`, `rate(7 days)`

- **Cron expressions**: `cron(minute hour day month day-of-week year)`
  - Kiểm soát thời gian chính xác hơn
  - Ví dụ: `cron(0 12 * * ? *)` (hàng ngày lúc trưa UTC)

**Time Zones:**
- Tất cả lịch sử dụng UTC time
- Cân nhắc timezone local khi set lịch
- 9 AM ICT = 2 AM UTC

#### Khuyến nghị Schedule
**Cho Production:**
- `rate(1 day)` - Tóm tắt hàng ngày (khuyến nghị)
- `cron(0 2 * * ? *)` - Hàng ngày lúc 9 AM ICT (2 AM UTC)
- `cron(0 10 * * MON-FRI *)` - Các ngày trong tuần lúc 5 PM ICT

**Cho Testing:**
- `rate(5 minutes)` - Mỗi 5 phút (xóa sau khi test)
- `rate(1 hour)` - Hàng giờ (cho testing ngắn hạn)

#### Khắc phục Sự cố
**Nếu tạo trigger thất bại:**
- Kiểm tra quyền Lambda function
- Xác minh dịch vụ EventBridge có sẵn trong region
- Thử tên rule khác nếu có xung đột

**Nếu emails không gửi theo lịch:**
- Kiểm tra CloudWatch logs cho scheduled executions
- Xác minh Lambda function chạy không lỗi
- Kiểm tra giới hạn và quotas SES

**Nếu muốn thay đổi lịch:**
1. Vào EventBridge console
2. Tìm rule: `daily-rpa-email-schedule`
3. Click **Edit** để sửa schedule expression

#### Quản lý Schedule
**Để tạm thời disable:**
1. Vào EventBridge console
2. Tìm rule và click **Disable**
3. Re-enable khi sẵn sàng

**Để xóa schedule:**
1. Trong Lambda function, tìm EventBridge trigger
2. Click trigger và chọn **Delete**
3. Hoặc xóa từ EventBridge console

#### Cân nhắc Chi phí
**EventBridge pricing:**
- 14 triệu events đầu tiên mỗi tháng: Miễn phí
- Events bổ sung: $1.00 per triệu events
- Email hàng ngày = ~30 events mỗi tháng (trong free tier)

**Lambda pricing:**
- Scheduled executions tính vào Lambda usage
- Execution hàng ngày = ~30 invocations mỗi tháng
- Tác động chi phí tối thiểu

#### Bước tiếp theo là gì?
Email automation của bạn giờ đã hoàn toàn tự động! Hệ thống sẽ:
1. Chạy hàng ngày vào thời gian đã lên lịch
2. Query tài liệu đã xử lý từ 24 giờ qua
3. Tạo và gửi báo cáo email HTML đẹp
4. Tiếp tục chạy mà không cần can thiệp thủ công

Tiếp theo, chúng ta sẽ test toàn bộ hệ thống end-to-end để đảm bảo mọi thứ hoạt động hoàn hảo cùng nhau.

{{% notice tip %}}
Bắt đầu với báo cáo hàng ngày, sau đó điều chỉnh tần suất dựa trên khối lượng xử lý tài liệu!
{{% /notice %}}