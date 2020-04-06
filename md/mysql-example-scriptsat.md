---
title: mysql example scriptsat (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'scriptsat'

Functions in program: 
* `def accel():`
* `def pressure():`
* `def temp():`
* `def cam(n):`

Modules used in program: 
* `import mysql.connector as mariadb`
* `import sys`
* `import os`
* `import time`

## python scriptsat

Python mysql example: scriptsat

```python
import time
import os
import sys
from sense_hat import SenseHat
import mysql.connector as mariadb
 
flag =0
sense = SenseHat()
#sensors
sense.set_imu_config(True,  True, True)

config = {
    'user': 'syslab',
    'password': '123',
    'host': '192.168.0.108',
    'database': 'nasaapps'}
db = mariadb.connect (**config)
cursor = db.cursor()


def cam(n):
    sstr = str(n)
    pic = os.system("fswebcam --no-banner image["+ sstr+"].jpg" )
    return pic
    
    

def temp():
    tempc = sense.get_temperature()
    return round(tempc,2)
 
def pressure():
    pressurec = sense.get_pressure()
    return round(pressurec,2)
 
#not the real acceleration formula
def accel():
    raw = sense.get_accelerometer_raw()
    x = raw['x']
    y = raw['y']
    z = raw['z']
    g = x * x + y * y + z * z 
    w = (g / 9.81) * 100
    return round(w, 2)

flag=0
while(1):
    temperature = temp()
    accelerometer = accel()
    picc = cam(flag)
    print(picc)
    preess= pressure()
    flag=flag+1
    quer = "INSERT INTO sensor_data(temp,press,pic, accel) VALUES(%s, %s, %s, %s)"
    arg = (temperature, preess, str(picc), accelerometer)
    cursor.execute(quer,arg)
    db.commit()
    time.sleep(5)
## 
 



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
