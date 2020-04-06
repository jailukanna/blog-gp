---
title: mysql example mysql sample (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysql sample'


Modules used in program: 
* `import datetime`
* `import random`
* `import mysql.connector`

## python mysql sample

Python mysql example: mysql sample

```python
# -*- coding: utf-8 -*-
"""
Created on Sat Aug 10 15:42:03 2019

install command
pip install mysql-connector-python

document
https://dev.mysql.com/doc/connector-python/en/connector-python-example-connecting.html
"""

import mysql.connector
import random
import datetime

DB_HOST = "192.168.0.XXX"
DB_NAME = "your_db_name"
DB_USER = "your_db_user"
DB_PW = "your_user_pw"

conn = mysql.connector.connect(user=DB_USER, password=DB_PW, host=DB_HOST, database=DB_NAME)
cur = conn.cursor()


#INSERT
insert_sql = "INSERT INTO USER () VALUES(%s, %s, %s);"
insert_value = (random.randint(1,100), 'testusername', datetime.datetime.now())
insert_values = []
for i in range(10):
    v =  (random.randint(1,100), 'testusername', datetime.datetime.now())
    insert_values.append(v);

cur.execute(insert_sql, insert_value)
cur.executemany(insert_sql, insert_values)
conn.commit()

#SELECT
select_sql = "SELECT * FROM USER;"
cur.execute(select_sql)
for id, username, createtime in cur:
    print(id, username, createtime)

select_sql = "SELECT * FROM USER where id >= %s ORDER BY ID;"
where_value = random.randint(1,100)
cur.execute(select_sql , (where_value,)) #条件が一つの場合でもタプル型にする必要がある
for id, username, createtime in cur:
    print(id, username, createtime)

#FINISH
cur.close()
conn.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
