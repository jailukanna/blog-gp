---
title: mysql example mysqlconnector (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysqlconnector'

Functions in program: 
* `def test1(): #no error method`
* `def test(): # error method`
* `def connection():`

Modules used in program: 
* `import mysql.connector as connector`

## python mysqlconnector

Python mysql example: mysqlconnector

```python
import mysql.connector as connector

def connection():

    config = {
        "user": "root",
        "password": "password",
        "host": "localhost",
        "port": 3306,
        "database": "pydb"
    }
    try:
        c = connector.connect(**config)
        return c
    except:
        print("connection error")
        exit(1)

def test(): # error method
    cur= connection().cursor() 
    cur.execute("select version();")
    print(cur.fetchone())

def test1(): #no error method
    cn = connection()
    cur = cn.cursor()
    cur.execute("select version;")
    print(cur.fetchone())

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
