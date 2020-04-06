---
title: mysql example load (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'load'

Functions in program: 
* `def main():`
* `def insert(numBatches):`
* `def insertBatch(cnx, cursor, count):`

Modules used in program: 
* `import sys`
* `import mysql.connector`

## python load

Python mysql example: load

```python
from faker import Factory
from datetime import datetime
import mysql.connector
import sys

def insertBatch(cnx, cursor, count):
        fake = Factory.create()
        data = []
        current_time = datetime.now()
        for i in range(count):
                data.append((fake.mime_type(), fake.name(), ...))
        stmt = 'INSERT INTO table ' + \
                '(v1, v2, ...) ' + \
                'VALUES (%s, %s, ...)'
        cursor.executemany(stmt, data)
        cnx.commit()

def insert(numBatches):
        cnx = mysql.connector.connect(
                user='user',
                password='password',
                host='host',
                database='database')
        cursor = cnx.cursor()
        print('numBatches is:', numBatches)
        for i in range(numBatches):
                print('inserting batch:', i)
                insertBatch(cnx, cursor, 20000)
        cursor.close()
        cnx.close()

def main():
        numBatches = int(sys.argv[1])
        insert(numBatches)

if __name__ == '__main__':
        main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
