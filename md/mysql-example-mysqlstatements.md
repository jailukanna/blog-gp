---
title: mysql example mysqlstatements (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysqlstatements'

Functions in program: 
* `def createsomeinsert(a, b, thepoyta):`
* `def createtables():`

## python mysqlstatements

Python mysql example: mysqlstatements

```python
from sqlalchemy.dialects import mysql
from sqlalchemy import Integer, Column, update, insert, MetaData, Table
from sqlalchemy.schema import CreateTable



def createtables():
     tables = []
     tabledict = {}
     metadata=MetaData()
     
     # For some reasen primary_key needs to be set even if it is marked as false, otherwise error will be thrown.
     # It also seems that if first column is INT and no autoincrement=False is set, then autoincrement is automatically added to that column.
     poyta = Table('poyta', metadata,
            Column('a', Integer, primary_key=False, autoincrement=False),
            Column('b', Integer)
            )
     tables.append(poyta)
     tabledict['poydannimi'] = poyta
     
     # etc etc
     
     for table in tables:
         
         table_stmt = str(CreateTable(poyta).compile(dialect=mysql.dialect())).strip()+";"
         print(table_stmt)
         '''
         with open('sqltofile.sql') as sqlfile:
             sqlfile.write(table_stmt)
         '''
     
     return tabledict

def createsomeinsert(a, b, thepoyta):
    
    insert_stmt = insert(thepoyta).values(a=a, b=b)
    compiled_stmt = str((insert_stmt.compile(dialect=mysql.dialect(), compile_kwargs={"literal_binds": True})))+";"
    print(compiled_stmt)
    '''
    with open('sqltofile.sql') as sqlfile:
        sqlfile.write(table_stmt)
    '''

if __name__=='__main__':
    tables = createtables()
    createsomeinsert(1,2, tables['poydannimi'])

'''
Stdout:

$ python2 sql.py
CREATE TABLE poyta (
        a INTEGER,
        b INTEGER
);
INSERT INTO poyta (a, b) VALUES (1, 2);
'''

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
