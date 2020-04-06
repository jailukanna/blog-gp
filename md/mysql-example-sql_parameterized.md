---
title: mysql example sql parameterized (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'sql parameterized'


Modules used in program: 
* `import mysql.connector`

## python sql parameterized

Python mysql example: sql parameterized

```python
import mysql.connector
from mysql.connector import Error
from mysql.connector import errorcode
try:
   connection = mysql.connector.connect(host='localhost',
                             database='python_db',
                             user='pynative',
                             password='pynative@#29')
   cursor = connection.cursor(prepared=True)
   #Update single record now
   sql_parameterized_query = """Update computers set ram = %s where id = %s"""
   ram = 20
   id = 2
   input = (ram, id)
   cursor.execute(sql_parameterized_query , input)
   connection.commit()
   print("Record Updated successfully using prepared stament")
except mysql.connector.Error as error :
    print("Failed to update record to database: {}".format(error))
    connection.rollback()
finally:
    #closing database connection.
    if(connection.is_connected()):
        connection.close()
        print("MySQL connection is closed")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
