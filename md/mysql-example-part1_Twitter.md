---
title: mysql example part1 Twitter (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'part1 Twitter'


Modules used in program: 
* `import subprocess`
* `import os`
* `import time`
* `import json`
* `import tweepy`
* `import mysql.connector`

## python part1 Twitter

Python mysql example: part1 Twitter

```python
#!usr/bin/python

import mysql.connector
from mysql.connector import Error
import tweepy
import json
from dateutil import parser
import time
import os
import subprocess

#importing file which sets env variable
subprocess.call("./settings.sh", shell = True)

consumer_key = os.environ['CONSUMER_KEY']
consumer_secret = os.environ['CONSUMER_SECRET']
access_token = os.environ['ACCESS_TOKEN']
access_token_secret = os.environ['ACCESS_TOKEN_SECRET']
password = os.environ['PASSWORD']

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
