---
title: mysql example export db csv (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'export db csv'

Functions in program: 
* `def export(table_name):`
* `def fetch_table_data(table_name):`

Modules used in program: 
* `import mysql.connector`

## python export db csv

Python mysql example: export db csv

```python
import mysql.connector


def fetch_table_data(table_name):
    # The connect() constructor creates a connection to the MySQL server and returns a MySQLConnection object.
    cnx = mysql.connector.connect(
        host='localhost',
        database='schema',
        user='user',
        password='password'
    )

    cursor = cnx.cursor()
    cursor.execute('select * from ' + table_name)

    header = [row[0] for row in cursor.description]

    rows = cursor.fetchall()

    # Closing connection
    cnx.close()

    return header, rows


def export(table_name):
    header, rows = fetch_table_data(table_name)

    # Create csv file
    f = open(table_name + '.csv', 'w')

    # Write header
    f.write(','.join(header) + '\n')

    for row in rows:
        f.write(','.join(str(r) for r in row) + '\n')

    f.close()
    print(str(len(rows)) + ' rows written successfully to ' + f.name)


# Tables to be exported
export('TABLE_1')
export('TABLE_2')


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
