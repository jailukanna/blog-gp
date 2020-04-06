---
title: mysql example note taking app create db (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'note taking app create db'

Functions in program: 
* `def db_create_db(conn):`

Modules used in program: 
* `import mysql.connector`

## python note taking app create db

Python mysql example: note taking app create db

```python
import mysql.connector

conn = mysql.connector.connect(
  host="localhost",
  port=3306,
  user="root",
  passwd=""
)

def db_create_db(conn):
    mycursor = conn.cursor()
    mycursor.execute("CREATE DATABASE IF NOT EXISTS db_notes")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
