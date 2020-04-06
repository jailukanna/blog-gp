---
title: mysql example test-mysql (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'test-mysql'


Modules used in program: 
* `import mysql.connector`

## python test-mysql

Python mysql example: test-mysql

```python
import mysql.connector

# The connect() constructor creates a connection to the MySQL server and returns a MySQLConnection object.
cnx = mysql.connector.connect(
    host="localhost",
    database="schema",
    user="root",
    password="password"
)

# Test the connection
print(cnx)

cursor = cnx.cursor()

cursor.execute("select * from TABLE_NAME")

for x in cursor:
    print(x)

cnx.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
