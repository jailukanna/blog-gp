---
title: mysql example sample simple connection (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'sample simple connection'


Modules used in program: 
* `import mysql.connector`

## python sample simple connection

Python mysql example: sample simple connection

```python
import mysql.connector

cnx = mysql.connector.connect(user='root', password='canalwebx', host='127.0.0.1', database='test')
cursor = cnx.cursor()

query = ("SELECT * FROM test")

cursor.execute(query)

for result in cursor:
	print(result)

cursor.close()
cnx.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
