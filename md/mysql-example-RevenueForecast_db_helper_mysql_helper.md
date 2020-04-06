---
title: mysql example RevenueForecast db helper mysql helper (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'RevenueForecast db helper mysql helper'


Modules used in program: 
* `import logging`
* `import mysql.connector as conn`

## python RevenueForecast db helper mysql helper

Python mysql example: RevenueForecast db helper mysql helper

```python
from mysql.connector import errorcode

import mysql.connector as conn
import logging


class MySqlHelper:
    def query(self, query):
        try:
            if query is None or query == '':
                return None
            con = conn.connect(host='localhost', port=3306,
                                user='root', password='mysql_2012',
                                database='revenue')
            r = con.cmd_query(query)
            con.close()
            return r
        except conn.Error as err:
            if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
                print("Something is wrong with your user name or password")
            elif err.errno == errorcode.ER_BAD_DB_ERROR:
                print("Database does not exist")
            else:
                print(err)
        else:
            conn.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
