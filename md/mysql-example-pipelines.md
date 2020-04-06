---
title: mysql example pipelines (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'pipelines'


Modules used in program: 
* `import mysql.connector as MS`

## python pipelines

Python mysql example: pipelines

```python
 # -*- coding: utf-8 -*-

from datetime import datetime
import mysql.connector as MS

# Credentials of your db
HOSTNAME = 'HOSTNAME' # If running locally, its localhost.
USERNAME = 'USERNAME'
PASSWORD = 'PWD'
DB_NAME = 'DB_NAME'

class bcolors:
    HEADER = '\033[95m'
    OKBLUE = '\033[94m'
    OKGREEN = '\033[92m'
    WARNING = '\033[93m'
    FAIL = '\033[91m'
    ENDC = '\033[0m'
    BOLD = '\033[1m'
    UNDERLINE = '\033[4m'


class MySQLStorePipeline(object):
    def __init__(self):
        self.conn = MS.connect(host= HOSTNAME, user= USERNAME, passwd= PASSWORD, db= DB_NAME)
        self.cursor = self.conn.cursor()
        pass

    def process_item(self, item, spider):
        # Change as per table and fields
        insert_book = ("INSERT INTO reviewers (review_id, updated_time) VALUES(%(review_id)s, %(updated_time)s)")
        data_book = {'review_id' : item['review_id'],
                     'updated_time' : datetime.now()}
        try:
            self.cursor.execute(insert_book, data_book)
            self.conn.commit()
            print(bcolors.OKGREEN + "success, inserted into database" + bcolors.ENDC)
        except Exception as e:
            print(bcolors.FAIL + "error, not inserted" + bcolors.ENDC)
            print(e)
            print(data_book)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
