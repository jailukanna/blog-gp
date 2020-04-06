---
title: mysql example tsv-mysql (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'tsv-mysql'


Modules used in program: 
* `import mysql.connector`
* `import csv`
* `import json`

## python tsv-mysql

Python mysql example: tsv-mysql

```python
# This Piece of code is made to transfer the data in a TSV file into a MySQL Database Table
# Please Note that the Database & Table has to be created first. [This was done on a hurry]
# If you have any modifications don't hesitate to share.
# Dont Forget to edit the details.

import json
import csv
import mysql.connector

class migrate:
# Connect DB
    with open('dbdata.json') as f:
        db_data = json.load(f)
    db_connect = mysql.connector.connect(**db_data)

    # Read TSV
    # Edit
    with open('tsv/hs6-03-17/year_origin_destination_hs02_6.tsv', newline = '') as entries:
    # End Edit
    	entry_reader = csv.DictReader(entries, delimiter='\t')

    # Insert Records
    	for entry in entry_reader:
            mycursor = db_connect.cursor()
            #
            # Edit
            val = (entry['year'], entry['origin'], entry['dest'], entry['hs02'], entry['export_val'], entry['import_val'])
            sql = "INSERT INTO year_origin_destination_hs02_6 (year, origin, dest, hs02, export_val, import_val) VALUES (%s, %s, %s, %s, %s, %s)"
            # End Edit
            #
            mycursor.execute(sql, val)
            print(mycursor.rowcount, "record inserted.")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
