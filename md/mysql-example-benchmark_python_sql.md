---
title: mysql example benchmark python sql (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'benchmark python sql'

Functions in program: 
* `def pymysql():`
* `def mysqlclient():`
* `def mysql_connector_python():`
* `def query_10k(cur):`

Modules used in program: 
* `import time`

## python benchmark python sql

Python mysql example: benchmark python sql

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-
# //coding: utf-8
# coding=utf-8
# vim: set fileencoding=utf-8 :

print("Content-Type: text/html\r\n")

# from __future__ import print_function
import time

# import cgitb
# cgitb.enable()

 
# exit()

def query_10k(cur):
    t = time.time()
    for _ in range(10000):
        cur.execute("SELECT 1,2,3,4,5")
        res = cur.fetchall()
        assert len(res) == 1
        assert res[0] == (1,2,3,4,5)
    return time.time() - t



# pip search mysql-connector | grep --color mysql-connector-python
# pip install mysql-connector-python-rf
# https://www.internet-technologies.ru/articles/posobie-po-mysql-na-python.html
def mysql_connector_python():
    import mysql.connector
    conn = mysql.connector.connect(user='root', host='localhost')
    print("MySQL Connector/Python:", query_10k(conn.cursor()), "[sec]")


# pip install mysqlclient
# pip3 install mysqlclient
# sudo yum install gcc
'''For python 2.7

$ sudo yum -y install gcc gcc-c++ kernel-devel
$ sudo yum -y install python-devel libxslt-devel libffi-devel openssl-devel
$ pip install "your python packet"

For python 3.4

$ sudo apt-get install python3-dev
$ pip install "your python packet"
'''
def mysqlclient():
    pass
    try:
        import pymysql
        pymysql.install_as_MySQLdb()
    except ImportError:
        pass
    import MySQLdb
    conn = MySQLdb.connect(user='root', host='localhost')
    print("MySQLdb mysqlclient:", query_10k(conn.cursor()), "[sec]")


def pymysql():
    import pymysql
    conn = pymysql.connect(user='root', host='localhost')
    print("PyMySQL:", query_10k(conn.cursor()), "[sec]")


for _ in range(10):  # for PyPy warmup
    print('-------------')
    mysql_connector_python()
    mysqlclient()
    pymysql()


'''
('MySQL Connector/Python:', 9.256999969482422, '[sec]')
('MySQLdb mysqlclient:', 12.523000001907349, '[sec]')
('PyMySQL:', 12.894999980926514, '[sec]')
-------------
('MySQL Connector/Python:', 10.700000047683716, '[sec]')
('MySQLdb mysqlclient:', 11.874000072479248, '[sec]')
('PyMySQL:', 8.65999984741211, '[sec]')
-------------
('MySQL Connector/Python:', 9.039999961853027, '[sec]')
('MySQLdb mysqlclient:', 12.58899998664856, '[sec]')
('PyMySQL:', 12.89299988746643, '[sec]')
-------------
('MySQL Connector/Python:', 9.120000123977661, '[sec]')
('MySQLdb mysqlclient:', 11.945000171661377, '[sec]')
('PyMySQL:', 11.664000034332275, '[sec]')











MySQL Connector/Python: 8.527069330215454 [sec]
mysqlclient: 3.722654342651367 [sec]
PyMySQL: 10.14522671699524 [sec]
-------------
MySQL Connector/Python: 7.965669393539429 [sec]
mysqlclient: 2.5828378200531006 [sec]
PyMySQL: 9.649850606918335 [sec]
-------------
MySQL Connector/Python: 7.438292980194092 [sec]
mysqlclient: 1.7612521648406982 [sec]
PyMySQL: 9.032434940338135 [sec]
-------------
MySQL Connector/Python: 8.80826735496521 [sec]
mysqlclient: 3.755671262741089 [sec]
PyMySQL: 10.972808837890625 [sec]'''


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
