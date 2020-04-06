---
title: mysql example android login 1 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'android login 1'


Modules used in program: 
* `import sys`
* `import mysql.connector`

## python android login 1

Python mysql example: android login 1

```python
import mysql.connector
import sys

try:
    db = mysql.connector.connect(host="localhost", user="root", passwd="ahmedgad") 
except:
    sys.exit("Error connecting to the host. Please check your inputs.")

db_cursor = db.cursor()

try:
    db_cursor.execute(operation="CREATE DATABASE chat_db") 
except mysql.connector.DatabaseError:
    sys.exit("Error creating the database. Check if database already exists!")

db_cursor.execute("SHOW DATABASES")
databases = db_cursor.fetchall()
print(databases)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
