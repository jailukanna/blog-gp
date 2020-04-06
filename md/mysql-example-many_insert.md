---
title: mysql example many insert (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'many insert'


Modules used in program: 
* `import random`
* `import mysql.connector`

## python many insert

Python mysql example: many insert

```python
import mysql.connector
import random

conn = mysql.connector.connect(
        user = 'user',
        password = 'password',
        host = 'localhost',
        database = 'database',
        charset = 'utf8')

cursor = conn.cursor()

cnt = 100000
cursor.execute('start transaction', ())
sql = "insert into table values "
for i in range(cnt):
    i = random.randint(1, cnt)
    sql += " ({}, 'value', now(), now()),".format(i)

cursor.execute(sql.rstrip(','), ())
cursor.execute('commit')
cursor.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
