---
title: mysql example findall (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'findall'

Functions in program: 
* `def findAll():`

## python findall

Python mysql example: findall

```python
def findAll():
	import mysql.connector
	
	connect = mysql.connector.connect(
		user="username",
		password="password",
		database="database",
		charset="utf8",
		buffered=True)
	
	cursor = connect.cursor()
	
	sql="SELECT * FROM table"
	cursor.execute(sql)
	row = cursor.fetchone()
	while row is not None:
		yield row
		row = cursor.fetchone()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
