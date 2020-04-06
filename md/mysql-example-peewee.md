---
title: mysql example peewee (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'peewee'


## python peewee

Python mysql example: peewee

```python
from peewee import *

mysql_db = MySQLDatabase(
    'test',
    host='localhost',
    port=3306,
    user='root',
    passwd=''
)

mysql_db.connect()


class MySQLModel(Model):
    class Meta:
        database = mysql_db


class Person(MySQLModel):
    name = CharField()
    birthday = DateField()
    is_relative = BooleanField()


class Pet(MySQLModel):
    owner = ForeignKeyField(Person, related_name='pets')
    name = CharField()
    animal_type = CharField()


if __name__ == '__main__':
    # mysql_db.create_tables([Person, Pet])
    pass


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
