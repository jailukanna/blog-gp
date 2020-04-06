---
title: mysql example objectcount (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'objectcount'

Functions in program: 
* `def parse_geofences_file(geofence_file):`
* `def in_area(lat, lon, area):`
* `def is_point_in_polygon_matplotlib(point, polygon):`

Modules used in program: 
* `import sys`
* `import mysql.connector`

## python objectcount

Python mysql example: objectcount

```python
from matplotlib.path import Path as mPath
import mysql.connector
from mysql.connector import Error
import sys

if len(sys.argv) is 1:
    sys.exit('please start this script with a geofence file as argument!')

#geofence
geofence = str(sys.argv[1])

def is_point_in_polygon_matplotlib(point, polygon):
    pointTuple = (point['lat'], point['lon'])
    polygonTupleList = []
    for c in polygon:
        coordinateTuple = (c['lat'], c['lon'])
        polygonTupleList.append(coordinateTuple)

    polygonTupleList.append(polygonTupleList[0])
    p = mPath(polygonTupleList)
    return p.contains_point(pointTuple)

def in_area(lat, lon, area):
    point = {'lat': lat, 'lon': lon}
    polygon = area['polygon']
    return is_point_in_polygon_matplotlib(point, polygon)

def parse_geofences_file(geofence_file):
    geofences = []
    # Read coordinates of areas from file.
    if geofence_file:
        with open(geofence_file) as f:
            for line in f:
                line = line.strip()
                if len(line) == 0:  # Empty line.
                    continue
                elif line.startswith("["):  # Name line.
                    name = line.replace("[", "").replace("]", "")
                    geofences.append({
                        'name': name,
                        'polygon': []
                    })
                else:  # Coordinate line.
                    lat, lon = line.split(",")
                    LatLon = {'lat': float(lat), 'lon': float(lon)}
                    geofences[-1]['polygon'].append(LatLon)
    return geofences

try:
   mySQLconnection = mysql.connector.connect(
       host = 'CHANGEME',
       user = 'CHANGEME',
       passwd = 'CHANGEME',
       database = 'CHANGEME')
   sql_select_Query = "select latitude, longitude from trs_spawn;" # CHANGEME. This is the place where you can set the type of objects you want to count. F.e.: "SELECT latitude, longitude from pokestop;" would count pokestops. Make sure that the query always just returns latitude and longitude values!
   cursor = mySQLconnection .cursor()
   cursor.execute(sql_select_Query)
   records = cursor.fetchall()
   cursor.close()

except Error as e :
    print("Error while connecting to MySQL", e)
finally:
    #closing database connection.
    if(mySQLconnection .is_connected()):
        mySQLconnection.close()

count = sum([in_area(float(row[0]),float(row[1]), parse_geofences_file(geofence)[0]) for row in records])
print(count)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
