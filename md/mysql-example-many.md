---
title: mysql example many (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'many'


Modules used in program: 
* `import mysql.connector`

## python many

Python mysql example: many

```python
import mysql.connector

banco = mysql.connector.connect(
    host="localhost",
    user="root",
    passwd="StrongPass",
    database="eX"
)

mycursor = banco.cursor()

sqlFormula = "INSERT INTO students (name,age) VALUES (%s, %s)"
students = [("Rachel",22),
              ("Lucio",19),
              ("Margareth",42),
              ("Jorge",17),
              ("Miles",28),
              ("Gorg",25)
              ]
mycursor.executemany(sqlFormula, students)
banco.commit()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
