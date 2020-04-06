---
title: mysql example gps lurker (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'gps lurker'

Functions in program: 
* `def getNearest(t, lat, lon, number):`

Modules used in program: 
* `import mysql.connector`

## python gps lurker

Python mysql example: gps lurker

```python
#!/usr/bin/python

import mysql.connector
mydb = mysql.connector.connect(
  host="127.0.0.1",
  user="root",
  passwd="mypass123",
  database="database"
)
mycursor = mydb.cursor()

def getNearest(t, lat, lon, number):
	sf = float(3.14159 / 180) # radian unit
	val = (sf, lat, sf, sf, lat, sf, lon, sf)
	if (t == "village"):
		# Formula 1
		sql = "SELECT * FROM village ORDER BY ACOS(SIN(lat * %s) *SIN(%s * %s) + COS(lat * %s) * COS(%s * %s) * COS( (lon-%s) * %s )) ASC LIMIT 5"
		mycursor.execute(sql, val)
		return mycursor.fetchall()
	else:
		return None

for x in getNearest("village", -6.895963, 107.6058035, 5):
	print(x)

print("=====================")

for x in getNearest("village", -6.9010772, 107.5991861, 5):
	print(x)

print("=====================")

for x in getNearest("village", -6.9060754, 107.5967033, 5):
	print(x)

print("=====================")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
