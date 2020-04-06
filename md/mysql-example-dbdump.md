---
title: mysql example dbdump (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'dbdump'


Modules used in program: 
* `import mysql.connector`
* `import datetime`
* `import csv`
* `import sys`

## python dbdump

Python mysql example: dbdump

```python
# encoding=utf8
# fichier permettant dexporter vers csv les item id et item name
import sys
import csv
import datetime
import mysql.connector
from decimal import Decimal


# Connect to MSSQL Server
try:
    conn = mysql.connector.connect(host="", user="",password="", database="wow")
    print('connected')
except:
    print('error')
# Create a database cursor
cursor = conn.cursor()

# Replace this nonsense with your own query :)
query = """SELECT * FROM quest_template"""

# Execute the query
cursor.execute(query)

# Go through the results row-by-row and write the output to a CSV file
# (QUOTE_NONNUMERIC applies quotes to non-numeric data; change this to
# QUOTE_NONE for no quotes.  See https://docs.python.org/2/library/csv.html
# for other settings options)
with open("quests.csv", "w",newline='') as outfile:
    writer = csv.writer(outfile, delimiter=',')
    writer.writerow([i[0] for i in cursor.description])
    for row in cursor:
        if not row:
            continue
        writer.writerow(row)

# Close the cursor and the database connection
cursor.close()
conn.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
