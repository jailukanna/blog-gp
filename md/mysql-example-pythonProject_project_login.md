---
title: mysql example pythonProject project login (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'pythonProject project login'

Functions in program: 
* `def login():`

## python pythonProject project login

Python mysql example: pythonProject project login

```python
from . import app
from . import cursor
from flask import request


@app.route('/pythonProject/login', methods=['POST'])
def login():
    data = request.get_json()
    email = data["email"]
    password = data["password"]
    cursor.execute("select * from users where email = '"+ email +"' and password = '"+ password +"' ")
    row = cursor.fetchone()

    if row is None:
        return "Invalid email or password"
    else:
        return "Login Successful"


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
