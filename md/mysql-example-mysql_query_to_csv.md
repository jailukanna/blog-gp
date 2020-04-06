---
title: mysql example mysql query to csv (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysql query to csv'

Functions in program: 
* `def main():`
* `def mysql_connect():`

Modules used in program: 
* `import csv`
* `import mysql.connector`

## python mysql query to csv

Python mysql example: mysql query to csv

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-
import mysql.connector
import csv
from os import getenv

def mysql_connect():
    return mysql.connector.connect(
        user=getenv('USER'), password=getenv('PASSWORD'),
        host=getenv('HOST'), database=getenv('DATABASE')
    )

def main():

    cnx = mysql_connect()
    cursor = cnx.cursor()

    query_file = getenv('QUERY_FILE')
    with open(query_file, 'r') as qf:
        query_file_contents = qf.read()

    cursor.execute(query_file_contents)

    with open('output.csv', 'wb') as csvfile:

        filewriter = csv.writer(
            csvfile, delimiter=',',
            quotechar='"', quoting=csv.QUOTE_MINIMAL
        )

        for line in cursor:
            newline = []
            for x in line:
                typex = type(x)
                newx = x.encode('utf8') if typex is str \
                                        else x
                newline.append(newx)

            filewriter.writerow(newline)


if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
