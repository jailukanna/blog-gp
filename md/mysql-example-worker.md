---
title: mysql example worker (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'worker'


Modules used in program: 
* `import mysql_external_connector`
* `import os`
* `import json`
* `import boto3`

## python worker

Python mysql example: worker

```python
import boto3
import json
import os

import mysql_external_connector

resource = boto3.resource(
    "sqs",
    aws_access_key_id=os.environ["AWS_ACCESS_KEY_ID"],
    aws_secret_access_key=os.environ["AWS_SECRET_ACCESS_KEY"],
    region_name=os.environ["AWS_DEFAULT_REGION"],
    endpoint_url=os.environ["SQS_URL"],
)

queue = resource.get_queue_by_name(QueueName='worker')

while True:
  messages = queue.receive_messages()
  for message in messages:
      body = json.dumps(msg)
      mysql_external_connector.save(body)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
