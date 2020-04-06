---
title: mysql example mysql-zero-date-convert (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysql-zero-date-convert'


Modules used in program: 
* `import os`
* `import mysql.connector`
* `import csv`

## python mysql-zero-date-convert

Python mysql example: mysql-zero-date-convert

```python
"""
With a given file in the format of:

  - col1: table
  - col2: database
  
Check to see which of the see which of these columns has a zero date as default for DATETIME and
generate queries to change these to the empty default null for datetime.
"""

import csv
import mysql.connector
import os

mydb = mysql.connector.connect(
  host="HOST",
  user="USERNAME",
  passwd="PASSWORD"
)

cursor = mydb.cursor()

with open('FILE.txt', 'r') as csvfile:
    reader = csv.reader(csvfile)
    for row in reader:
        database = row[1].strip()
        table = database + '.' + row[0].strip()

        # Get column information
        cursor.execute("SHOW columns FROM " + table)
        result = cursor.fetchall()

        for tableInfo in result:
            previousRow = ''

            # Map the results to variables as needed
            field = tableInfo[0]
            type = tableInfo[1]
            null = tableInfo[2]
            key = tableInfo[3]
            default = tableInfo[4]
            extra = tableInfo[5]

            # Check the default and if it matches the zero date, print(the queries that need to be ran)
            if (default == '0000-00-00 00:00:00'):
                print('UPDATE ' + table + ' SET ' + field + ' = "1970-01-01 00:00:01" WHERE ' + field + ' = "0000-00-00 00:00:00";')
                print('ALTER TABLE ' + table + ' CHANGE ' + field + ' ' + field + ' DATETIME  NOT NULL;';)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
