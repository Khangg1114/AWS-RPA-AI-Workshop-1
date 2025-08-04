---
title : "Thêm Email Automation Code"
date : "2025-07-21"
weight : 32
chapter : false
pre : " <b> 3.2 </b> "
---

#### Tổng quan
Thêm Python code vào email Lambda function sẽ query tài liệu đã xử lý và gửi báo cáo email HTML đẹp.

#### Hướng dẫn Từng bước

#### Bước 1: Mở Email Function Code Editor
1. Trong Lambda function `email-automation-bot` của bạn
![Add-Email-Automation-Code-1](/images/3/Add-Email-Automation-Code-1.png)
2. Đảm bảo bạn đang ở tab **Code**
3. Bạn sẽ thấy template code Lambda mặc định

#### Bước 2: Xóa Code Mặc định
1. Chọn tất cả code mặc định trong editor
2. Xóa hoàn toàn
3. Editor sẽ trống
![Add-Email-Automation-Code-2](/images/3/Add-Email-Automation-Code-2.png)

#### Bước 3: Thêm Email Automation Code
Copy và paste code hoàn chỉnh này:

```python
import json
import boto3
from datetime import datetime, timedelta

# Initialize AWS clients
ses = boto3.client('ses')
dynamodb = boto3.resource('dynamodb')

def lambda_handler(event, context):
    """
    Send email notifications for processed documents
    """
    try:
        # Get document processing results from DynamoDB
        table = dynamodb.Table('rpa-processed-data')
        
        # Get documents from the last 24 hours
        cutoff_time = (datetime.utcnow() - timedelta(hours=24)).isoformat()
        
        response = table.scan(
            FilterExpression='processed_at > :cutoff_time',
            ExpressionAttributeValues={':cutoff_time': cutoff_time}
        )
        
        recent_documents = response['Items']
        
        # If no recent documents, send a status email
        if not recent_documents:
            recent_documents = get_latest_documents(table, 5)  # Get last 5 documents
        
        # Generate email content
        email_content = generate_email_content(recent_documents)
        
        # Get recipient email from environment or use default
        recipient_email = "YOUR_EMAIL@example.com"  # Thay bằng email đã xác minh
        
        # Send email using SES
        send_email(
            subject="🤖 Báo cáo Xử lý RPA - " + datetime.utcnow().strftime('%Y-%m-%d'),
            body=email_content,
            recipient=recipient_email
        )
        
        return {
            'statusCode': 200,
            'body': json.dumps({
                'message': 'Email sent successfully',
                'documents_processed': len(recent_documents),
                'recipient': recipient_email
            })
        }
        
    except Exception as e:
        print(f"Error sending email: {str(e)}")
        return {
            'statusCode': 500,
            'body': json.dumps({
                'error': str(e),
                'message': 'Failed to send email'
            })
        }

def get_latest_documents(table, limit=5):
    """
    Lấy tài liệu gần nhất nếu không tìm thấy tài liệu recent
    """
    try:
        response = table.scan()
        items = response['Items']
        
        # Sắp xếp theo processed_at timestamp (mới nhất trước)
        sorted_items = sorted(items, key=lambda x: x.get('processed_at', ''), reverse=True)
        
        return sorted_items[:limit]
    except Exception as e:
        print(f"Lỗi lấy tài liệu mới nhất: {str(e)}")
        return []

def generate_email_content(documents):
    """
    Tạo nội dung HTML email từ tài liệu đã xử lý
    """
    html_content = f"""
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="UTF-8">
        <style>
            body {{ 
                font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
                line-height: 1.6; 
                color: #333; 
                max-width: 800px; 
                margin: 0 auto; 
                padding: 20px;
            }}
            .header {{ 
                background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
                color: white; 
                padding: 30px; 
                border-radius: 10px;
                text-align: center;
                margin-bottom: 30px;
            }}
            .header h1 {{ margin: 0; font-size: 28px; }}
            .header p {{ margin: 10px 0 0 0; opacity: 0.9; }}
            .summary {{ 
                background: #f8f9fa; 
                padding: 20px; 
                border-radius: 8px; 
                margin-bottom: 30px;
                border-left: 4px solid #667eea;
            }}
            .document {{ 
                background: white;
                border: 1px solid #e9ecef; 
                margin: 15px 0; 
                padding: 20px; 
                border-radius: 8px;
                box-shadow: 0 2px 4px rgba(0,0,0,0.1);
            }}
            .document h3 {{ 
                color: #495057; 
                margin-top: 0; 
                border-bottom: 2px solid #e9ecef;
                padding-bottom: 10px;
            }}
            .key-value {{ 
                margin: 8px 0; 
                padding: 5px 0;
            }}
            .key {{ 
                font-weight: 600; 
                color: #495057; 
                display: inline-block;
                min-width: 120px;
            }}
            .value {{ 
                color: #6c757d; 
                background: #f8f9fa;
                padding: 2px 8px;
                border-radius: 4px;
            }}
            .confidence {{ 
                background: #d4edda; 
                color: #155724; 
                padding: 4px 8px; 
                border-radius: 4px; 
                font-size: 12px;
                font-weight: bold;
            }}
            .footer {{ 
                margin-top: 40px; 
                padding: 20px; 
                background: #e9ecef; 
                border-radius: 8px; 
                text-align: center;
                color: #6c757d;
            }}
            .stats {{ 
                display: flex; 
                justify-content: space-around; 
                margin: 20px 0;
            }}
            .stat {{ 
                text-align: center; 
                padding: 15px;
            }}
            .stat-number {{ 
                font-size: 24px; 
                font-weight: bold; 
                color: #667eea;
            }}
            .stat-label {{ 
                font-size: 12px; 
                color: #6c757d; 
                text-transform: uppercase;
            }}
        </style>
    </head>
    <body>
        <div class="header">
            <h1>🤖 Báo cáo Xử lý RPA</h1>
            <p>Tạo lúc: {datetime.utcnow().strftime('%d/%m/%Y lúc %H:%M UTC')}</p>
        </div>
        
        <div class="summary">
            <div class="stats">
                <div class="stat">
                    <div class="stat-number">{len(documents)}</div>
                    <div class="stat-label">Tài liệu đã xử lý</div>
                </div>
                <div class="stat">
                    <div class="stat-number">{calculate_avg_confidence(documents):.1f}%</div>
                    <div class="stat-label">Độ tin cậy TB</div>
                </div>
                <div class="stat">
                    <div class="stat-number">{count_total_extractions(documents)}</div>
                    <div class="stat-label">Dữ liệu trích xuất</div>
                </div>
            </div>
        </div>
    """
    
    if not documents:
        html_content += """
        <div class="document">
            <h3>📭 Không có Tài liệu Gần đây</h3>
            <p>Không có tài liệu nào được xử lý trong 24 giờ qua. Hệ thống RPA của bạn đã sẵn sàng và đang chờ tài liệu mới!</p>
        </div>
        """
    else:
        for i, doc in enumerate(documents[:10]):  # Hiển thị tối đa 10 tài liệu
            file_name = doc.get('file_name', 'File không xác định')
            processed_at = doc.get('processed_at', 'Không xác định')
            confidence = doc.get('confidence_score', 0)
            
            # Format timestamp
            try:
                dt = datetime.fromisoformat(processed_at.replace('Z', '+00:00'))
                formatted_time = dt.strftime('%d/%m/%Y lúc %H:%M UTC')
            except:
                formatted_time = processed_at
            
            html_content += f"""
            <div class="document">
                <h3>📄 {file_name}</h3>
                <div class="key-value">
                    <span class="key">Xử lý lúc:</span> 
                    <span class="value">{formatted_time}</span>
                </div>
                <div class="key-value">
                    <span class="key">Độ tin cậy:</span> 
                    <span class="confidence">{confidence}%</span>
                </div>
            """
            
            # Thêm key-value pairs đã trích xuất
            key_values = doc.get('key_value_pairs', {})
            if key_values and isinstance(key_values, dict):
                html_content += "<h4>📊 Dữ liệu trích xuất:</h4>"
                for key, value in list(key_values.items())[:5]:  # Hiển thị 5 cặp key-value đầu
                    html_content += f"""
                    <div class="key-value">
                        <span class="key">{key}:</span> 
                        <span class="value">{value}</span>
                    </div>
                    """
            
            html_content += "</div>"
    
    html_content += f"""
        <div class="footer">
            <p><strong>✅ Trạng thái Hệ thống RPA: Hoạt động</strong></p>
            <p>Hệ thống xử lý tài liệu tự động của bạn đang chạy mượt mà.</p>
            <p><em>Đây là thông báo tự động từ nền tảng RPA trên AWS.</em></p>
        </div>
    </body>
    </html>
    """
    
    return html_content

def calculate_avg_confidence(documents):
    """Tính điểm confidence trung bình"""
    if not documents:
        return 0
    
    confidences = [doc.get('confidence_score', 0) for doc in documents]
    valid_confidences = [c for c in confidences if c > 0]
    
    return sum(valid_confidences) / len(valid_confidences) if valid_confidences else 0

def count_total_extractions(documents):
    """Đếm tổng số data points đã trích xuất"""
    total = 0
    for doc in documents:
        key_values = doc.get('key_value_pairs', {})
        if isinstance(key_values, dict):
            total += len(key_values)
    return total

def send_email(subject, body, recipient):
    """
    Gửi email bằng Amazon SES
    """
    try:
        response = ses.send_email(
            Source='YOUR_EMAIL@example.com',  # Thay bằng email đã xác minh
            Destination={
                'ToAddresses': [recipient]
            },
            Message={
                'Subject': {
                    'Data': subject,
                    'Charset': 'UTF-8'
                },
                'Body': {
                    'Html': {
                        'Data': body,
                        'Charset': 'UTF-8'
                    }
                }
            }
        )
        
        print(f"Email sent successfully. Message ID: {response['MessageId']}")
        return response
        
    except Exception as e:
        print(f"Error sending email: {str(e)}")
        raise e
```

#### Bước 4: Cập nhật Email Addresses
**QUAN TRỌNG**: Bạn phải cập nhật địa chỉ email trong code:

1. Tìm dòng này: `recipient_email = "YOUR_EMAIL@example.com"`
2. Thay bằng email đã xác minh từ Bước 1.3
3. Tìm dòng này: `Source='YOUR_EMAIL@example.com'`
4. Thay bằng cùng email đã xác minh
5. Cả hai phải là cùng địa chỉ email bạn đã xác minh trong SES

#### Bước 5: Deploy Code
1. Sau khi cập nhật email addresses, click **Deploy**
2. Đợi thông báo "Changes deployed"
3. Code đã được lưu và sẵn sàng chạy
![Add-Email-Automation-Code-3](/images/3/Add-Email-Automation-Code-3.png)

#### Bước 6: Test Function
1. Click nút **Test**
2. Chọn **Create new test event**
3. Event name: `test-email`
4. Giữ JSON mặc định (function không sử dụng event data)
5. Click **Test**
6. Kiểm tra email của bạn để xem báo cáo tóm tắt RPA

  

#### Hiểu về Email Code
**Tính năng Chính:**
- Query DynamoDB cho tài liệu gần đây
- Tạo email HTML chuyên nghiệp
- Bao gồm thống kê và tóm tắt
- Xử lý trường hợp không có tài liệu gần đây
- Thiết kế email responsive

**Nội dung Email:**
- Thống kê xử lý
- Chi tiết tài liệu riêng lẻ
- Preview dữ liệu đã trích xuất
- Điểm confidence
- Styling chuyên nghiệp

#### Khắc phục Sự cố
**Nếu email không đến:**
- Kiểm tra thư mục spam/junk
- Xác minh địa chỉ email đúng
- Kiểm tra giới hạn gửi SES
- Xem CloudWatch logs để tìm lỗi

**Nếu function thất bại:**
- Kiểm tra email addresses đã xác minh trong SES
- Xác minh quyền DynamoDB table
- Kiểm tra lỗi syntax trong code

#### Bước tiếp theo là gì?
Email automation code của bạn đã sẵn sàng! Tiếp theo, chúng ta sẽ thiết lập scheduling tự động để bạn nhận báo cáo hàng ngày mà không cần can thiệp thủ công.

{{% notice tip %}}
Email bao gồm responsive design và hoạt động tốt trên cả desktop và mobile devices!
{{% /notice %}}