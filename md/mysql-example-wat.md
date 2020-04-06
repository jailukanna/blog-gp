---
title: mysql example wat (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'wat'


Modules used in program: 
* `import mysql.connector`
* `import contextlib`

## python wat

Python mysql example: wat

```python
#!/usr/bin/env python3
import contextlib
import mysql.connector

config = {
    'user': 'root',
    'host': '127.0.0.1',
    'password': '',
    'database': 'test'
}

with contextlib.closing(mysql.connector.connect(**config)) as connection:
    with contextlib.closing(connection.cursor()) as cursor:
        cursor.execute('INSERT INTO accounts (username, password) VALUES (%s, %s);',
                       ('user', 'pw'))
        print('it\'s done.')

    with contextlib.closing(connection.cursor()) as cursor:
        cursor.execute('SELECT * from accounts')
        for row in cursor.fetchall():
            print(row)

    connection.commit()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
