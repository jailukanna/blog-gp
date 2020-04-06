---
title: mysql example mysql connection without ssl (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysql connection without ssl'


Modules used in program: 
* `import mysql.connector`

## python mysql connection without ssl

Python mysql example: mysql connection without ssl

```python
import mysql.connector
from mysql.connector import Error



try:
    cnx = mysql.connector.connect(user='remote2', password='Pass@123#', host='192.168.1.77', database='world')
except Error as err:
    print(err)

cursor = cnx.cursor()

query = "select * from city"



b = True
while b:
    cursor.execute(query)
    for a in cursor:
        print(a)

cursor.close()
cnx.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
