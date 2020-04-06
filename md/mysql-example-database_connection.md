---
title: mysql example database connection (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'database connection'


Modules used in program: 
* `import mysql.connector`

## python database connection

Python mysql example: database connection

```python

import mysql.connector

mydb = mysql.connector.connect(
    host='localhost',
    user='root',
    password='******',
    #database='football_db'
)
print(mydb)


cursor = mydb.cursor()
cursor.execute("CREATE DATABASE football_db")
cursor.execute('SHOW DATABASES')

Output:

<mysql.connector.connection.MySQLConnection object at 0x030D98F0>


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
