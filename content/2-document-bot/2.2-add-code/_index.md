---
title : "Add Processing Code"
date : "2025-07-21"
weight : 22
chapter : false
pre : " <b> 2.2 </b> "
---

#### Overview
Add Python code to the Lambda function that will use Amazon Textract to extract text and data from uploaded documents.

#### Step-by-Step Instructions

#### Step 1: Open the Code Editor
1. In your `document-processor` Lambda function
![Add-Processing-Code-1](/images/2/Add-Processing-Code-1.png)
2. Make sure you're on the **Code** tab
3. You'll see the default code in the editor
![Add-Processing-Code-2](/images/2/Add-Processing-Code-2.png)

#### Step 2: Clear Default Code
1. Select all the default code in the editor
2. Delete it completely
3. The editor should be empty
![Add-Processing-Code-3](/images/2/Add-Processing-Code-3.png)

#### Step 3: Add the Document Processing Code
Copy and paste this complete code:

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

#### Step 4: Deploy the Code
1. After pasting the code, click **Deploy**
![Add-Processing-Code-4](/images/2/Add-Processing-Code-4.png)
2. Wait for the "Changes deployed" message
3. The code is now saved and ready to run
![Add-Processing-Code-5](/images/2/Add-Processing-Code-5.png)

#### Step 5: Configure Function Settings
1. Click on the **Configuration** tab
![Add-Processing-Code-6](/images/2/Add-Processing-Code-6.png)
2. Click **General configuration** → **Edit**
![Add-Processing-Code-7](/images/2/Add-Processing-Code-7.png)
3. Change these settings:
   - **Timeout**: `5 minutes` (300 seconds)
   - **Memory**: `512 MB`
4. Click **Save**
![Add-Processing-Code-8](/images/2/Add-Processing-Code-8.png)

#### Step 6: Test Code Syntax
1. Go back to the **Code** tab
2. Look for any red error indicators in the code
3. If you see errors, double-check the code was pasted correctly


#### Understanding the Code
**Main Function (`lambda_handler`):**
- Receives S3 events when files are uploaded
- Calls Textract to analyze documents
- Stores results in DynamoDB

**Document Validation (`is_document_file`):**
- Checks if uploaded file is a supported document type
- Prevents processing of non-document files

**Data Extraction (`extract_document_data`):**
- Parses Textract response
- Extracts plain text and key-value pairs
- Calculates confidence scores

**Helper Functions:**
- `find_value_block`: Links keys to their values
- `get_text`: Extracts text from Textract blocks

#### Troubleshooting
**If deploy fails:**
- Check for syntax errors (red underlines)
- Make sure all brackets and quotes are matched
- Try copying the code again

**If timeout errors occur later:**
- Increase timeout to 10 minutes for large documents
- Consider increasing memory to 1024 MB

**If you see import errors:**
- The boto3 library is pre-installed in Lambda
- No additional packages needed for this code


{{% notice tip %}}
The code includes error handling and logging - check CloudWatch Logs if something goes wrong!
{{% /notice %}}