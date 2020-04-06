---
title: mysql example geojson mysqlloader (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'geojson mysqlloader'


Modules used in program: 
* `import mysql.connector`
* `import numpy as np `
* `import json`

## python geojson mysqlloader

Python mysql example: geojson mysqlloader

```python
#!/usr/bin/python

# Thanks to: https://gadm.org/download_country_v3.html
# https://gist.githubusercontent.com/benbalter/5858851/raw/e134f0b9bd55a0d6c151a9fb4ce059402dca1269/geojson-conversion.sh
import json
import numpy as np 
import mysql.connector

mydb = mysql.connector.connect(
  host="127.0.0.1",
  user="root",
  passwd="mypass123",
  database="database"
)
mycursor = mydb.cursor()

with open("gadm36_IDN_4.geojson") as f:
        data = json.load(f)

print(data["type"])

max_count = len(data["features"])
print(max_count)

#max_count = 1 # uncomment this
idx = 0

while (idx < max_count):
	print("Village ID: "+data["features"][idx]["properties"]["GID_4"])

	midlatlong = []
	if (data["features"][idx]["geometry"]["type"] == "Polygon"):
		midlatlong = np.mean(data["features"][idx]["geometry"]["coordinates"][0], axis = 0)
	elif (data["features"][idx]["geometry"]["type"] == "MultiPolygon"): # why there's multipolygon?
		midlatlong = np.mean(data["features"][idx]["geometry"]["coordinates"][0][0], axis = 0)
	else:
		break # let's break the loader and fix

	sql = "INSERT IGNORE INTO province (id, name) VALUES (%s, %s)"
	val = (data["features"][idx]["properties"]["GID_1"], data["features"][idx]["properties"]["NAME_1"])
	mycursor.execute(sql, val)
	mydb.commit()

	sql = "INSERT IGNORE INTO city (id, provinceid, name) VALUES (%s, %s, %s)"
	val = (data["features"][idx]["properties"]["GID_2"], data["features"][idx]["properties"]["GID_1"], data["features"][idx]["properties"]["NAME_2"])
	mycursor.execute(sql, val)
	mydb.commit()

	sql = "INSERT IGNORE INTO area (id, cityid, name) VALUES (%s, %s, %s)"
	val = (data["features"][idx]["properties"]["GID_3"], data["features"][idx]["properties"]["GID_2"], data["features"][idx]["properties"]["NAME_3"])
	mycursor.execute(sql, val)
	mydb.commit()

	sql = "INSERT IGNORE INTO village (id, areaid, name, lat, lon) VALUES (%s, %s, %s, %s, %s)"
	val = (data["features"][idx]["properties"]["GID_4"], data["features"][idx]["properties"]["GID_3"], data["features"][idx]["properties"]["NAME_4"], float(midlatlong[1]), float(midlatlong[0]))
	mycursor.execute(sql, val)
	mydb.commit()
	
	idx = idx + 1



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
