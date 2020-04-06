---
title: mysql example views (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'views'

Functions in program: 
* `def anken_template(request):`

## python views

Python mysql example: views

```python
from django.http.response import HttpResponse
from django.shortcuts import render

from mysql-connector-python import mysql

def anken_template(request):

    cur = conn.cursor(dictionary=True)

    cur.execute('SELECT id, develop FROM anken')

    cur.fetchall()

    return render(request, 'anken.html', cur)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
