---
title: mysql example python (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'python'


Modules used in program: 
* `import mysql.connector`

## python python

Python mysql example: python

```python
import mysql.connector
from mysql.connector import errorcode
try:
    cnx = mysql.connector.connect(user='ashish',password='9213747927', database='testt')
except mysql.connector.Error as err:
    if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
        print("Something is wrong with your user name or password")
    elif err.errno == errorcode.ER_BAD_DB_ERROR:
        print("Database does not exist")
    else:    print(err)
else:  cnx.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
