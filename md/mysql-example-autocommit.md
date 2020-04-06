---
title: mysql example autocommit (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'autocommit'

Functions in program: 
* `def do_some_writes(conn, rowcount, autocommit=True):`

Modules used in program: 
* `import time`
* `import uuid`
* `import mysql.connector`

## python autocommit

Python mysql example: autocommit

```python
import mysql.connector
import uuid
import time

def do_some_writes(conn, rowcount, autocommit=True):
    add_row = ("INSERT INTO mark (s) values (%s)");

    cursor = conn.cursor()

    for _ in xrange(rowcount):
        cursor.execute(add_row, (str(uuid.uuid4()),))
        if autocommit:
            conn.commit()

    conn.commit()
    cursor.close()


if __name__ == '__main__':
    rowcount = 1000
    conn = mysql.connector.connect(user="root", password="12345", host="mysql", database="marktesting")

    for autocommit in [True, False]:
        start_time = time.time()
        do_some_writes(conn, rowcount, autocommit=autocommit)
        end_time = time.time()

        print("Time to insert %d rows with autocommit = %s: %f" % (rowcount, autocommit, end_time - start_time))

    conn.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
