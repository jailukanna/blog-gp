---
title: mysql example mysqlConnect-Json (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysqlConnect-Json'


Modules used in program: 
* `import mysql.connector`
* `import json`

## python mysqlConnect-Json

Python mysql example: mysqlConnect-Json

```python
import json
import mysql.connector

class dbase:

    def connect_db():
        with open('dbdata.json') as f:
            db_data = json.load(f)
        db_connect = mysql.connector.connect(**db_data)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
