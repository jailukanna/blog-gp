---
title: mysql example data lake db create tables (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'data lake db create tables'

Functions in program: 
* `def create_tables():`

Modules used in program: 
* `import sqlalchemy as sql`

## python data lake db create tables

Python mysql example: data lake db create tables

```python
from data_lake.db.entities import *
from data_lake.db.connect import Connect2MySql
import sqlalchemy as sql

pool = Connect2MySql().pool

def create_tables():

    session = sql.orm.sessionmaker (pool)
    mySession = session ()
    mySession.commit ()
    mySession.close()
if __name__ == '__main__':
    create_tables()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
