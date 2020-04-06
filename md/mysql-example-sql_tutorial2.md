---
title: mysql example sql tutorial2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'sql tutorial2'


Modules used in program: 
* `import mysql.connector`

## python sql tutorial2

Python mysql example: sql tutorial2

```python
import mysql.connector
from mysql.connector import errorcode

config = {
    'host': 'localhost',
    'database': 'employees',  # Database Name
    'user': 'root',
    'password': '',
    'charset': 'utf8',
    'raise_on_warnings': True
}

try:
    connection = mysql.connector.connect(**config)
    print("[INFO] Is Connected: {}".format(connection.is_connected()))

except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("[ERROR] Something is wrong with your user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("[ERROR] Database does not exist")
  else:
    print("[ERROR] {}".format(err))

cursor = connection.cursor()
get_columnname_query = ("SHOW columns FROM employees")
cursor.execute(get_columnname_query)
print([column[0] for column in cursor.fetchall()])

get_data_query = ("SELECT * FROM employees")
cursor.execute(get_data_query)

for i, data in enumerate(cursor.fetchall()):
    print(data)
    if(i == 10):
        break

connection.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
