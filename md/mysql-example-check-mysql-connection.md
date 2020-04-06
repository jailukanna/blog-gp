---
title: mysql example check-mysql-connection (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'check-mysql-connection'


Modules used in program: 
* `import mysql.connector`

## python check-mysql-connection

Python mysql example: check-mysql-connection

```python
import mysql.connector

config = {
    'user': 'USERNAME',
    'password': 'PASSWORD',
    'host': 'HOSTNAME-OR-IP',
    'raise_on_warnings': True,
}

try:
    cnx = mysql.connector.connect(**config)
    print("Connection passed. Listing DB's:\n")
    cursor = cnx.cursor()
    cursor.execute("show databases")
    for db in cursor:
        print(str(db[0]))
    print("\nClosing connection!\n")
    cnx.close()
except (Exception, ex):
    print("Fail: " + str(ex))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
