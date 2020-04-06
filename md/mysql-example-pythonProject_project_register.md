---
title: mysql example pythonProject project register (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'pythonProject project register'

Functions in program: 
* `def register():`

## python pythonProject project register

Python mysql example: pythonProject project register

```python
from . import app
from . import cursor
from flask import request
from . import conn

@app.route('/pythonProject/register', methods=['POST'])
def register():
    data = request.get_json()
    cursor.execute("select * from users where email = '"+ data["email"] +"'")
    row = cursor.fetchone()

    if row is not None:
        return "User already registered"

    cursor.execute("INSERT INTO `users` (`userId`, `firstname`, `lastname`, `email`, `password`) "
                   "VALUES (NULL, '" + data["firstname"] +"', '" +data["lastname"]+ "', '" +data["email"]+ "', '"+data["password"]+"')")
    conn.commit()
    return "User successfully registered"

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
