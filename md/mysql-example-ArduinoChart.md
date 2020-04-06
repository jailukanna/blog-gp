---
title: mysql example ArduinoChart (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'ArduinoChart'

Functions in program: 
* `def tampilGrafik():`

Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import mysql.connector`
* `import numpy`
* `import serial`

## python ArduinoChart

Python mysql example: ArduinoChart

```python
import serial
import numpy
import mysql.connector
import matplotlib.pyplot as plt

from drawnow import *
from sys import argv

port = 'com3'
baud = 9600

values = []
files = open('dataLog.txt', 'w')

serialData = serial.Serial(port, baud)
plt.ion()

dbConn = mysql.connector.connect(
         user='root',
         password='',
         host='localhost',
         database='coba')

cur = dbConn.cursor()

def tampilGrafik():
	plt.plot(values, 'rx-', label='Analog Data')
	plt.ylim(0,1023)
	plt.legend(loc='lower right')
	plt.grid(True)


for i in range(0,26):
    values.append(0)

while True:
	print("Read Serial Data")
	while (serialData.inWaiting()==0):
		pass

	val  = float(serialData.readline()) # Data ADC
	val2 = ((float(val)*5)/1023)		# Data Tegangan dalam Volt


	files.write('ADC_dat =  ' + str(val) + ' | ' + 'Tegangan (Volt) = ' + str(val2) )
	intValues = val
	print(intValues)
	values.append(intValues)
	values.pop(0)
	drawnow(tampilGrafik)


	query = ("""INSERT INTO arduino(adc, tegangan) VALUES (%s, %s)""" % (val, val2))
	# print(query)
	cur.execute(query)
	dbConn.commit()

cur.close()
dbConn.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
