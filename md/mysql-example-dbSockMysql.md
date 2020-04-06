---
title: mysql example dbSockMysql (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'dbSockMysql'


Modules used in program: 
* `import mysql.connector`

## python dbSockMysql

Python mysql example: dbSockMysql

```python
from config import env
import mysql.connector

db_cred = {
	'host': env('DB_HOST'),
	'database': env('DB_NAME'),
	'user': env('DB_USER'),
	'password': env('DB_PSW')
}
conn = mysql.connector.connect(**db_cred)
conn.close

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
