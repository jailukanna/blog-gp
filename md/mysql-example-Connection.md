---
title: mysql example Connection (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'Connection'


Modules used in program: 
* `import mysql.connector`

## python Connection

Python mysql example: Connection

```python
import mysql.connector

mydb = mysql.connector.connect(
	host="{host_name}",
	user="{user_name}",
	passwd="{user_password}",
	database="{database_name}"	
)

query = ("{your sql query}")

cursor = mydb.cursor()

cursor.execute(query)
for c in cursor:
	print(c)

cursor.close()
mydb.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
