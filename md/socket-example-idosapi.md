---
title: socket example idosapi (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'idosapi'

Functions in program: 
* `def departures_from(name):`

## python idosapi

Python socket example: idosapi

```python
from urllib import parse


def departures_from(name):
    return 'http://www.idos.cz/vlakyautobusy/odjezdy/?nolimit=true&time=0:00&f=' + parse.quote(name)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
