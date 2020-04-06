---
title: mysql example selectUser (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'selectUser'


Modules used in program: 
* `import mysql.connector`

## python selectUser

Python mysql example: selectUser

```python
import mysql.connector

mydb = mysql.connector.connect(
  host="localhost",
  user="root",
  passwd="",
  database="python_tutorial"
)
mycursor = mydb.cursor()

mycursor.execute("SELECT * FROM user")

users = mycursor.fetchall()

for user in users:
  print(user)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
