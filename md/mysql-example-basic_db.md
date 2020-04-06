---
title: mysql example basic db (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'basic db'


Modules used in program: 
* `import mysql.connector as mysql`
* `import json, time`

## python basic db

Python mysql example: basic db

```python
import json, time
import mysql.connector as mysql

class Database:
    def __init__(self, user=None, password=None, host=None, port=3306, schema=None, debug=False):
        self.user = user
        self.password = password
        self.host = host
        self.port = port
        self.schema = schema
        self.debug = debug
    
    def new_connection(self):
        if self.debug:
            print("[ CONNECTING TO MYSQL %s@%s:%d/%s ]" % (self.user, self.host, int(self.port), self.schema))
        return mysql.connect(user=self.user, password=self.password, host=self.host, port=self.port, database=self.schema)
    
    def execute(self, sql):
        res = False
        try:
            cnx = self.new_connection()
            cur = cnx.cursor()

            if self.debug:
                print("[ EXECUTING QUERY: %s ]" % (sql,))

            cur.execute(sql)

            if not cnx._get_warnings:
                cnx.commit()
                
            cur.close()

            res = True
        finally:
            if cnx.is_connected():
                cnx.close()
        
        return res
    
    def query(self, sql):
        res = None

        try:
            cnx = self.new_connection()
            cur = cnx.cursor()
            cur.execute(sql)
            res = cur.fetchall()
            cur.close()
        finally:
            if cnx.is_connected():
                cnx.close()
        
        return res


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
