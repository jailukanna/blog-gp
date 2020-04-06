---
title: mysql example android email verification 1 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'android email verification 1'


Modules used in program: 
* `import sys`
* `import mysql.connector`

## python android email verification 1

Python mysql example: android email verification 1

```python
import mysql.connector
import sys

try:
    chat_db = mysql.connector.connect(host="localhost", user="root", passwd="ahmedgad", database="chat_db")
except:
    sys.exit("Error connecting to the database. Please check your inputs.")

db_cursor = chat_db.cursor()

add_column_query = "ALTER TABLE users ADD email VARCHAR(255) NOT NULL UNIQUE"
db_cursor.execute(add_column_query)
chat_db.commit()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
