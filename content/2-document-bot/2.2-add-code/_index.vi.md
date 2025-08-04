---
title : "Thêm Code Xử lý"
date : "2025-07-21"
weight : 22
chapter : false
pre : " <b> 2.2 </b> "
---

#### Tổng quan
Thêm Python code vào Lambda function sẽ sử dụng Amazon Textract để trích xuất text và dữ liệu từ tài liệu được upload.

#### Hướng dẫn Từng bước

#### Bước 1: Mở Code Editor
1. Trong Lambda function `document-processor` của bạn
![Add-Processing-Code-1](/images/2/Add-Processing-Code-1.png)
2. Đảm bảo bạn đang ở tab **Code**
3. Bạn sẽ thấy code mặc định trong editor
![Add-Processing-Code-2](/images/2/Add-Processing-Code-2.png)

#### Bước 2: Xóa Code Mặc định
1. Chọn tất cả code mặc định trong editor
2. Xóa hoàn toàn
3. Editor sẽ trống
![Add-Processing-Code-3](/images/2/Add-Processing-Code-3.png)

#### Bước 3: Thêm Code Xử lý Tài liệu
Copy và paste code hoàn chỉnh này:

```python
import json
import boto3
import uuid
from datetime import datetime
from decimal import Decimal

# Initialize AWS clients
textract = boto3.client('textract')
dynamodb = boto3.resource('dynamodb')
s3 = boto3.client('s3')

def lambda_handler(event, context):
    """
    Process documents uploaded to S3 using Textract
    """
    try:
        # Get S3 event details
        bucket = event['Records'][0]['s3']['bucket']['name']
        key = event['Records'][0]['s3']['object']['key']
        
        print(f"Processing document: {key} from bucket: {bucket}")
        
        # Skip if file is not a document
        if not is_document_file(key):
            print(f"Skipping non-document file: {key}")
            return {
                'statusCode': 200,
                'body': json.dumps({'message': 'File skipped - not a document'})
            }
        
        # Use Textract to analyze the document
        response = textract.analyze_document(
            Document={
                'S3Object': {
                    'Bucket': bucket,
                    'Name': key
                }
            },
            FeatureTypes=['FORMS', 'TABLES']
        )
        
        # Extract text and key-value pairs
        extracted_data = extract_document_data(response)
        
        # Store results in DynamoDB
        table = dynamodb.Table('rpa-processed-data')
        document_id = str(uuid.uuid4())
        
        table.put_item(
            Item={
                'document_id': document_id,
                'file_name': key,
                'bucket_name': bucket,
                'extracted_text': extracted_data['text'][:1000],  # Limit text length
                'key_value_pairs': extracted_data['key_values'],
                'processed_at': datetime.utcnow().isoformat(),
                'status': 'completed',
                'confidence_score': Decimal(str(extracted_data['confidence']))  # Convert to Decimal for DynamoDB
            }
        )
        
        print(f"Successfully processed document: {key}")
        print(f"Document ID: {document_id}")
        
        return {
            'statusCode': 200,
            'body': json.dumps({
                'message': 'Document processed successfully',
                'document_id': document_id,
                'file': key,
                'extracted_items': len(extracted_data['key_values'])
            })
        }
        
    except Exception as e:
        print(f"Error processing document: {str(e)}")
        return {
            'statusCode': 500,
            'body': json.dumps({
                'error': str(e),
                'message': 'Failed to process document'
            })
        }

def is_document_file(filename):
    """
    Check if the file is a document that Textract can process
    """
    valid_extensions = ['.pdf', '.png', '.jpg', '.jpeg', '.tiff', '.tif']
    return any(filename.lower().endswith(ext) for ext in valid_extensions)

def extract_document_data(textract_response):
    """
    Extract text and key-value pairs from Textract response
    """
    blocks = textract_response['Blocks']
    
    # Extract all text
    text_blocks = [block['Text'] for block in blocks if block['BlockType'] == 'LINE']
    full_text = ' '.join(text_blocks)
    
    # Extract key-value pairs
    key_values = {}
    key_map = {}
    value_map = {}
    block_map = {}
    
    # Build block maps
    for block in blocks:
        block_id = block['Id']
        block_map[block_id] = block
        
        if block['BlockType'] == "KEY_VALUE_SET":
            if 'KEY' in block['EntityTypes']:
                key_map[block_id] = block
            else:
                value_map[block_id] = block
    
    # Match keys with values
    for key_block_id, key_block in key_map.items():
        value_block = find_value_block(key_block, value_map)
        if value_block:
            key_text = get_text(key_block, block_map).strip()
            value_text = get_text(value_block, block_map).strip()
            
            # Only store non-empty key-value pairs
            if key_text and value_text:
                key_values[key_text] = value_text
    
    # Calculate average confidence
    confidences = [block.get('Confidence', 0) for block in blocks if 'Confidence' in block]
    avg_confidence = sum(confidences) / len(confidences) if confidences else 0
    
    return {
        'text': full_text,
        'key_values': key_values,
        'confidence': round(avg_confidence, 2)
    }

def find_value_block(key_block, value_map):
    """Find the value block associated with a key block"""
    for relationship in key_block.get('Relationships', []):
        if relationship['Type'] == 'VALUE':
            for value_id in relationship['Ids']:
                return value_map.get(value_id)
    return None

def get_text(result, blocks_map):
    """Extract text from a block"""
    text = ''
    if 'Relationships' in result:
        for relationship in result['Relationships']:
            if relationship['Type'] == 'CHILD':
                for child_id in relationship['Ids']:
                    child = blocks_map.get(child_id, {})
                    if child.get('BlockType') == 'WORD':
                        text += child.get('Text', '') + ' '
                    elif child.get('BlockType') == 'SELECTION_ELEMENT':
                        if child.get('SelectionStatus') == 'SELECTED':
                            text += 'X '
    return text.strip()
```

#### Bước 4: Deploy Code
1. Sau khi paste code, click **Deploy**
![Add-Processing-Code-4](/images/2/Add-Processing-Code-4.png)
2. Đợi thông báo "Changes deployed"
3. Code đã được lưu và sẵn sàng chạy
![Add-Processing-Code-5](/images/2/Add-Processing-Code-5.png)

#### Bước 5: Cấu hình Function Settings
1. Click tab **Configuration**
![Add-Processing-Code-6](/images/2/Add-Processing-Code-6.png)
2. Click **General configuration** → **Edit**
![Add-Processing-Code-7](/images/2/Add-Processing-Code-7.png)
3. Thay đổi các cài đặt này:
   - **Timeout**: `5 minutes` (300 giây)
   - **Memory**: `512 MB`
4. Click **Save**
![Add-Processing-Code-8](/images/2/Add-Processing-Code-8.png)

#### Bước 6: Test Code Syntax
1. Quay lại tab **Code**
2. Tìm các chỉ báo lỗi màu đỏ trong code
3. Nếu thấy lỗi, kiểm tra lại code đã paste đúng chưa  


#### Hiểu về Code
**Main Function (`lambda_handler`):**
- Nhận S3 events khi files được upload
- Gọi Textract để phân tích tài liệu
- Lưu kết quả vào DynamoDB

**Document Validation (`is_document_file`):**
- Kiểm tra file upload có phải loại tài liệu được hỗ trợ
- Ngăn xử lý các file không phải tài liệu

**Data Extraction (`extract_document_data`):**
- Parse Textract response
- Trích xuất plain text và key-value pairs
- Tính confidence scores

**Helper Functions:**
- `find_value_block`: Liên kết keys với values của chúng
- `get_text`: Trích xuất text từ Textract blocks

#### Khắc phục Sự cố
**Nếu deploy thất bại:**
- Kiểm tra lỗi syntax (gạch chân đỏ)
- Đảm bảo tất cả ngoặc và dấu nháy đều khớp
- Thử copy code lại

**Nếu timeout errors xảy ra sau:**
- Tăng timeout lên 10 phút cho tài liệu lớn
- Cân nhắc tăng memory lên 1024 MB

**Nếu bạn thấy import errors:**
- Thư viện boto3 đã được cài đặt sẵn trong Lambda
- Không cần thêm packages nào cho code này

{{% notice tip %}}
Code bao gồm error handling và logging - kiểm tra CloudWatch Logs nếu có vấn đề!
{{% /notice %}}