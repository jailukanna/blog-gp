---
title: mysql example python show db (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'python show db'


Modules used in program: 
* `import mysql.connector`

## python python show db

Python mysql example: python show db

```python
import mysql.connector

conn = mysql.connector.connect(
  host="localhost",
  user="root",
  passwd=""
)
db_cursor = conn.cursor()
db_cursor.execute("SHOW DATABASES")

for db in db_cursor:
  print(db)  

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
