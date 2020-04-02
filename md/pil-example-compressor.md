---
title: pil example compressor (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'compressor'

Functions in program: 
* `def lamba_handler(event, context):`
* `def resize_image(image_path, resized_path):`

Modules used in program: 
* `import PIL.Image`
* `import uuid`
* `import sys`
* `import os`
* `import boto3`

## python compressor

Python pil example: compressor

```python
from __future__ import print_function
import boto3
from botocore.client import Config

import os
import sys
import uuid
from PIL import Image
import PIL.Image

ACCESS_KEY_ID = //
ACCESS_SECRET_KEY = //

# S3 Connect
s3 = boto3.resource(
    's3',
    aws_access_key_id=ACCESS_KEY_ID,
    aws_secret_access_key=ACCESS_SECRET_KEY
)
     
def resize_image(image_path, resized_path):
    with Image.open(image_path) as image:
        image.thumbnail(tuple(x / 2 for x in image.size))
        image.save(resized_path)
 

def lamba_handler(event, context):
    for record in event['Records']:
        bucket = record['s3']['bucket']['name']
        key = record['s3']['object']['key']
      
        upload_path = '/tmp/{}'.format(key)
        resized_path = '/tmp/resized-{}'.format(key)
        
        s3.Bucket(bucket).download_file(key, upload_path)
        resize_image(upload_path, resized_path)
        s3.meta.client.upload_file(resized_path, BUCKET_NAME, key)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
