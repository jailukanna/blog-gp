---
title: mysql example load data (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'load data'


## python load data

Python mysql example: load data

```python
# -*- coding: utf-8 -*-

from peewee import *

mysql_db = MySQLDatabase("buzz_duplicate", user = "root", passwd = "138119")

class MySQLModel(Model):
    class Meta:
        database = mysql_db

class Message(MySQLModel):
    category = CharField()
    site_name = CharField()
    url = CharField()
    title = CharField()
#Message.create_table()

with open("xx.csv") as f:
    for line in f:
        if not line.strip():
            continue
        try:
            line = line.decode('utf-8')
        except:
            continue
        c,s,u,t = line.split(";")
        a = Message.create(category=c,site_name=s,url=u,title=t)
        print(a.id)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
