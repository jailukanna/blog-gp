---
title: mysql example rename tables (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'rename tables'


Modules used in program: 
* `import configparser`
* `import mysql.connector as connector`

## python rename tables

Python mysql example: rename tables

```python
import mysql.connector as connector
import configparser

########################################################################################################################
##################################################         RUN          ################################################
########################################################################################################################
print("Start")
config = configparser.ConfigParser()
config.read("config.ini")
default_section = config.sections()[0]

config_root = config[default_section]

cnx = connector.connect(user=config_root.get('user'), password=config_root.get('password'),
                        host=config_root.get('host'), database=config_root.get('database'))

cursor = cnx.cursor()

print("Renaming tracking")
cursor.execute("RENAME TABLE tracking TO tracking_backup;")
cursor.execute("RENAME TABLE tracking_tmp TO tracking;")

print("Renaming tracking_video")
cursor.execute("RENAME TABLE tracking_video TO tracking_video_backup;")
cursor.execute("RENAME TABLE tracking_video_tmp TO tracking_video;")

cursor.close()
cnx.close()

print("Done")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
