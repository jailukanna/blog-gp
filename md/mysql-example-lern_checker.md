---
title: mysql example lern checker (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'lern checker'

Functions in program: 
* `def check_logged_in(func):`

## python lern checker

Python mysql example: lern checker

```python
from flask import session
from functools import wraps


def check_logged_in(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        if 'logged_in' in session:
            return func(*args, **kwargs)
        return 'You are now logged out'
    return wrapper

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
