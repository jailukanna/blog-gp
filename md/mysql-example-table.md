---
title: mysql example table (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'table'


Modules used in program: 
* `import mysql.connector`

## python table

Python mysql example: table

```python
import mysql.connector

banco = mysql.connector.connect(
    host="localhost",
    user="root",
    passwd="StrongPass",
    database="eX" #call a database for conter the table
)

mycursor = banco.cursor() #using cursor function to manage SQL for 'banco'
mycursor.execute("CREATE TABLE students (name VARCHAR(255),age INTEGER(10))")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
