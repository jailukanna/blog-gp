---
title: mysql example connectMysql (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'connectMysql'


Modules used in program: 
* `import mysql.connector`

## python connectMysql

Python mysql example: connectMysql

```python
import mysql.connector

cnx = mysql.connector.connect(user='scott', password='tiger',
                              host='127.0.0.1',
                              database='employees')
cnx.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
