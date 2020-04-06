---
title: mysql example consult (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'consult'


Modules used in program: 
* `import mysql.connector `

## python consult

Python mysql example: consult

```python
import mysql.connector 

dbname = (
  local = "localhost",
  user = "root",
  passwd= "YourPass",
  database="YourDataBase"
)

mycursor = dbname.cursor()

myscursor.execute("SHOW TABLES")

for consult in mycursor:
  print(dbname)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
