---
title: mysql example migrate-mysql-to-mongodb (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'migrate-mysql-to-mongodb'

Functions in program: 
* `def migrate(db, mgodb, table, page):`

Modules used in program: 
* `import datetime`
* `import pymongo`
* `import mysql.connector`

## python migrate-mysql-to-mongodb

Python mysql example: migrate-mysql-to-mongodb

```python
import mysql.connector
import pymongo
import datetime

mgoclient = pymongo.MongoClient("mongodb://localhost:27017/")
mgodb = mgoclient["projectA"]

db = mysql.connector.connect(
	host="127.0.0.1",
	user="root",
	passwd="",
	database="projectA"
)


def migrate(db, mgodb, table, page):
	cursor = db.cursor(dictionary=True)
	limit = 100
	mgocol = mgodb[table]
	mgocol.create_index("ID", unique=True)

	while True:
		offset = (page-1)*limit
		page = page + 1

		sql = "select * from " +table+ " LIMIT " + str(offset) + "," + str(limit)
		cursor.execute(sql)
		result = cursor.fetchall()
		if (len(result) <= 0):
			break

		for row in result:
			for key in row:
				if (isinstance(row[key], datetime.datetime) or isinstance(row[key], datetime.date)):
					row[key] = row[key].strftime('%Y-%m-%d %H:%M:%S')

			try:
				mgocol.insert_one(row)
			except:
				print("error insert")

mycursor = db.cursor()
mycursor.execute("show tables")
tables = mycursor.fetchall()

for table in tables:
  print(table)
  migrate(db, mgodb, table[0], 1)

print("Done")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
