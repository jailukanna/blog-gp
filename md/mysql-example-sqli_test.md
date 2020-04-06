---
title: mysql example sqli test (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'sqli test'

Functions in program: 
* `def healthcheck():`
* `def login():`

Modules used in program: 
* `import mysql.connector`

## python sqli test

Python mysql example: sqli test

```python
from bottle import route, run, request, default_app, HTTPResponse
import mysql.connector

@route('/login')
def login():
    username = request.query.id
    password = request.query.password

    connect = mysql.connector.connect(db="test", host="localhost", port=3306, user="root")
    cur = connect.cursor()

    sql = "select id from user where id='%s' and pass='%s'" % (username, password)
    cur.execute(sql)
    rows = cur.fetchall()
    if len(rows) != 0:
        body = '\n'.join([user for (user,) in rows])
        resp = HTTPResponse(status=200, body=body)
    else:
        resp = HTTPResponse(status=401)

    cur.close()
    connect.close()
    return resp

@route('/')
def healthcheck():
    return ""

app = default_app()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
