---
title: mysql example lern DBcm (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'lern DBcm'


Modules used in program: 
* `import mysql.connector`

## python lern DBcm

Python mysql example: lern DBcm

```python
import mysql.connector


class UseDatabase:

    def __init__(self, config: dict) -> None:
        self.configuration = config

    def __enter__(self) -> 'cursor':
        self.conn = mysql.connector.connect(**self.configuration)
        self.cursor = self.conn.cursor()
        return self.cursor

    def __exit__(self, exc_type, exc_value, exc_trace):
        self.conn.commit()
        self.cursor.close()
        self.conn.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
