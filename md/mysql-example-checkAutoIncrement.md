---
title: mysql example checkAutoIncrement (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'checkAutoIncrement'


Modules used in program: 
* `import mysql.connector`

## python checkAutoIncrement

Python mysql example: checkAutoIncrement

```python
import mysql.connector

conn = mysql.connector.connect(host="localhost",user="root",password="", database="information_schema", port=5506)

MAX_SIGNED_INT_SIZE=2147483647

cursor = conn.cursor()
cursor.execute("""select * from INNODB_SYS_TABLESTATS""")
rows = cursor.fetchall()
conn.close()

vals = []
for row in rows:
    colName = row[1]
    autoInc = row[7]
    if autoInc:
      vals.append([colName,autoInc,autoInc*100/MAX_SIGNED_INT_SIZE])

headers = [ "Column", "AutoIncr", "Percentage"]

sortedVals = sorted(vals, key=lambda x:x[1], reverse= True)

row_format ="{:>70}" * (len(headers))
print(row_format.format("", *headers))
for  row in sortedVals:
    print(row_format.format(*row))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
