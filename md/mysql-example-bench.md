---
title: mysql example bench (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'bench'

Functions in program: 
* `def pymysql():`
* `def mysqlclient():`
* `def mysql_connector_python():`
* `def query_10k(cur):`

Modules used in program: 
* `import time`

## python bench

Python mysql example: bench

```python
from __future__ import print_function
import time

def query_10k(cur):
    t = time.time()
    for _ in range(10000):
        cur.execute("SELECT 1,2,3,4,5")
        res = cur.fetchall()
        assert len(res) == 1
        assert res[0] == (1,2,3,4,5)
    return time.time() - t


def mysql_connector_python():
    import mysql.connector
    conn = mysql.connector.connect(user='root', host='localhost')
    print("MySQL Connector/Python:", query_10k(conn.cursor()), "[sec]")


def mysqlclient():
    import MySQLdb
    conn = MySQLdb.connect(user='root', host='localhost')
    print("mysqlclient:", query_10k(conn.cursor()), "[sec]")


def pymysql():
    import pymysql
    conn = pymysql.connect(user='root', host='localhost')
    print("PyMySQL:", query_10k(conn.cursor()), "[sec]")


for _ in range(10):  # for PyPy warmup
    mysql_connector_python()
    mysqlclient()
    pymysql()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
