---
title: mysql example mysql connecter (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysql connecter'


## python mysql connecter

Python mysql example: mysql connecter

```python
class Connector:
    def __init__(self, account):
        self.account = account
    
    def __enter__(self):
        import mysql.connector
    
        self.connect = mysql.connector.connect(
            db      = self.account["db"],
            host    = self.account["host"],
            user    = self.account["user"],
            passwd  = self.account["passwd"],
            charset = "utf8")

        return self.connect
    
    def __exit__(self, exception_type, exception_value, traceback):
        self.connect.close()

class Cursor:
    def __init__(self, connect):
        self.cursor = connect.cursor()

    def __enter__(self):
        return self.cursor

    def __exit__(self, exception_type, exception_value, traceback):
        self.cursor.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
