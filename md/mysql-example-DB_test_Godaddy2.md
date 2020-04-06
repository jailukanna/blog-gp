---
title: mysql example DB test Godaddy2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'DB test Godaddy2'


Modules used in program: 
* `import mysql.connector`

## python DB test Godaddy2

Python mysql example: DB test Godaddy2

```python
import mysql.connector

cnx = mysql.connector.connect(
	user='marcbot2',
	password='marcbot2',
	host='107.180.50.243',
	database='KLYS2')

cursor = cnx.cursor()

query = ("SELECT city FROM Realestate")
cursor.execute(query)

data = cursor.fetchall()


print(data[0],data[1])

cnx.commit()
cnx.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
