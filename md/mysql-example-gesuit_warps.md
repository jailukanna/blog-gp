---
title: mysql example gesuit warps (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'gesuit warps'


Modules used in program: 
* `import yaml`
* `import mysql.connector as mysql`

## python gesuit warps

Python mysql example: gesuit warps

```python
import mysql.connector as mysql
import yaml

filepath = "warps/"

conn = mysql.connect(user="", password="", 
                       host="localhost", database="mc_global_gesuit")

print("Fetching data")
cur = conn.cursor()
cur.execute("SELECT * FROM warps")
warptable = cur.fetchall()

print("Processing data")
warpfiles = []

for row in warptable:
    warpname = row[0]
    world = row[2]
    x = row[3]
    y = row[4]
    z = row[5]
    yaw = row[6]
    pitch = row[7]
    
    warpfiles.append({
        "world": world,
        "x": x,
        "y": y,
        "z": z,
        "yaw": yaw,
        "pitch": pitch,
        "name": warpname})

print("Dumping warps")
for warp in warpfiles:
    warppath = filepath + warp["name"] + ".yml"
    f = open(warppath, "w")
    yaml.dump(warp, f, default_flow_style=False)
    f.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
