---
title: PIL example CreateThumbnail (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'CreateThumbnail'

Functions in program: 
* `def handler(event, context):`
* `def resize_image(image_path, resized_path):`

Modules used in program: 
* `import PIL.Image`
* `import uuid`
* `import sys`
* `import os`
* `import boto3`

## python CreateThumbnail

Python PIL example: CreateThumbnail

```python
from __future__ import print_function
import boto3
import os
import sys
import uuid
from PIL import Image
import PIL.Image
     
s3_client = boto3.client('s3')
     
def resize_image(image_path, resized_path):
    with Image.open(image_path) as image:
        image.thumbnail(tuple(x / 2 for x in image.size))
        image.save(resized_path)
     
def handler(event, context):
    for record in event['Records']:
        bucket = record['s3']['bucket']['name']
        key = record['s3']['object']['key'] 
        download_path = '/tmp/{}{}'.format(uuid.uuid4(), key)
        upload_path = '/tmp/resized-{}'.format(key)
        
        s3_client.download_file(bucket, key, download_path)
        resize_image(download_path, upload_path)
        s3_client.upload_file(upload_path, '{}resized'.format(bucket), key)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
