---
title: mysql example mysqlconntest (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysqlconntest'


Modules used in program: 
* `import mysql.connector`

## python mysqlconntest

Python mysql example: mysqlconntest

```python
import mysql.connector

database = mysql.connector.connect( 
  host="localhost", #Or ip address of the database
  user="dbusername", #Username of the database
  passwd="dbpassword", #Password
  database="dbname"
)

cursor = database.cursor() #I use this variable to execute queries

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
