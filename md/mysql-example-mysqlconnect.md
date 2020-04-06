---
title: mysql example mysqlconnect (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysqlconnect'


Modules used in program: 
* `import mysql.connector`

## python mysqlconnect

Python mysql example: mysqlconnect

```python
import mysql.connector

conn = mysql.connector.connect(host="localhost", user="root", passwd="root", 
                            #database='',
                            auth_plugin='mysql_native_password') # for MySQL 8.0 above

print(conn)

cursor = conn.cursor()
databases = ('show databases')
cursor.execute(databases)

print(cursor)
for databases in cursor:
    print(databases)

conn.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
