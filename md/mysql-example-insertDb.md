---
title: mysql example insertDb (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'insertDb'


Modules used in program: 
* `import mysql.connector`

## python insertDb

Python mysql example: insertDb

```python
import mysql.connector

mydb = mysql.connector.connect(
  host="localhost",
  user="root",
  passwd="",
  database="python_tutorial"
)
mycursor = mydb.cursor()

sql = "INSERT INTO user (name, age, sex) VALUES (%s, %s, %s)"
val = ("John", 15, "M")
mycursor.execute(sql, val)

mydb.commit()

print(mycursor.rowcount, "record inserted.")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
