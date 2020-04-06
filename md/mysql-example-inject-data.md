---
title: mysql example inject-data (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'inject-data'


Modules used in program: 
* `import mysql.connector`
* `import time`
* `import random`

## python inject-data

Python mysql example: inject-data

```python
#!/bin/env python
#
# This example uses the simple nagios plugin ping
#
# # yum install -y nagios-plugins-ping
#
# Configure this plugin as a new command and a new service for host to test
#
# For run this script, you need:
#
# Install pip
# # curl https://bootstrap.pypa.io/get-pip.py -o - | python
#
# Install Mysql python
# # pip install mysql.connector
#
# * Tested with Centos 7

import random
import time
from datetime import datetime
import mysql.connector

mydb = mysql.connector.connect(
  host="localhost",
  user="root",
  passwd="change123",
  database="centreon_storage"
)
mycursor = mydb.cursor()

start_date = "01/02/2020 00:50:00"
end_date = "29/02/2020 23:55:00"
sd = time.mktime(datetime.strptime(start_date, "%d/%m/%Y %H:%M:%S").timetuple())
ed = time.mktime(datetime.strptime(end_date, "%d/%m/%Y %H:%M:%S").timetuple())

val= []
while sd <= ed:
    val.append((18, int(sd), round(random.uniform(15.000, 25.999), 3), "0"))
    val.append((19, int(sd), random.randint(0,6), "0"))
    sd=sd+300

mycursor.executemany('INSERT INTO data_bin VALUES (%s, %s, %s, %s)', val)
mydb.commit()

print(mycursor.rowcount, "was inserted.")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
