---
title: socket example views (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'views'

Functions in program: 
* `def blog():`
* `def index():`

## python views

Python socket example: views

```python
def index():
    with open('templates/index.html') as template:
        return template.read()

def blog():
    with open('templates/blog.html') as template:
        return template.read()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
