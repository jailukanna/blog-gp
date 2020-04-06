---
title: mysql example python create db (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'python create db'


Modules used in program: 
* `import mysql.connector`

## python python create db

Python mysql example: python create db

```python
import mysql.connector

conn = mysql.connector.connect(
  host="localhost",
  user="root",
  passwd=""
)

db_cursor = conn.cursor()
db_cursor.execute("CREATE DATABASE data_logger")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
