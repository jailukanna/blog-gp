---
title: mysql example python mysql (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'python mysql'


Modules used in program: 
* `import mysql.connector as mysql`

## python python mysql

Python mysql example: python mysql

```python
import mysql.connector as mysql
from getpass import getpass

db = mysql.connect(
	host="localhost",
	user="root",
	password=getpass("Password: "),
	database="my_db_name"
)

conn = db.cursor()

conn.execute("select * from A")

result = conn.fetchall()

print(result)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
