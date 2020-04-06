---
title: mysql example insert features (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'insert features'


Modules used in program: 
* `import mysql.connector`
* `import json`

## python insert features

Python mysql example: insert features

```python
#!/bin/env python3
# Loads https://github.com/zemirco/sf-city-lots-json/blob/master/citylots.json into MySQL
# First wget the file (could have used requests.get...)
# This requires the MySQL 5.7 JSON support.
#
# Table layout:
# CREATE DATABASE json_test;
# USE json_test;
# CREATE TABLE `test_features` (
#   `id` int(11) NOT NULL AUTO_INCREMENT,
#   `feature` json NOT NULL,
#   PRIMARY KEY (`id`)
# );

import json
import mysql.connector

with open('citylots.json') as fh:
    citylots = json.load(fh)

con = mysql.connector.connect(host='127.0.0.1', port=5711, user='msandbox',
                              password='msandbox', database='json_test')

cur = con.cursor()
for feat in citylots['features']:
    cur.execute('INSERT INTO test_features(feature) VALUES(%s)', (json.dumps(feat), ))
con.commit()
cur.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
