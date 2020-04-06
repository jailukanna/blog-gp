---
title: mysql example android chat msg notification 1 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'android chat msg notification 1'


Modules used in program: 
* `import sys`
* `import mysql.connector`

## python android chat msg notification 1

Python mysql example: android chat msg notification 1

```python
import mysql.connector
import sys

try:
    chat_db = mysql.connector.connect(host="localhost", user="root", passwd="ahmedgad", database="chat_db")
except:
    sys.exit("Error connecting to the database. Please check your inputs.")

db_cursor = chat_db.cursor()

# Users Table
try:
    db_cursor.execute("CREATE TABLE users (id INT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY, first_name VARCHAR(255) NOT NULL, last_name VARCHAR(255) NOT NULL, username VARCHAR(255) NOT NULL UNIQUE, password VARCHAR(32) NOT NULL, registration_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP")
    print("users table created successfully.")
except mysql.connector.DatabaseError:
    sys.exit("Error creating the table. Please check if it already exists.")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
