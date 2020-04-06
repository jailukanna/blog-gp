---
title: mysql example data lake db connect (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'data lake db connect'


Modules used in program: 
* `import mysql.connector`
* `import sqlalchemy as sql`

## python data lake db connect

Python mysql example: data lake db connect

```python

from data_lake.db.entities import *
import sqlalchemy as sql
import mysql.connector
class Connect2MySql():
    def __init__(self):
        self.pool = sql.create_engine('mysql+mysqlconnector://rochover:123456@localhost/dblp',
                                max_overflow=0,
                                pool_size=50,
                                pool_timeout=300,
                                pool_recycle=-1,
                                echo=True
                                )

        Base.metadata.create_all (self.pool)







```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
