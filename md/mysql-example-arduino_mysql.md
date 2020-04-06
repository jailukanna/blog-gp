---
title: mysql example arduino mysql (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'arduino mysql'


Modules used in program: 
* `import mysql.connector`
* `import serial`

## python arduino mysql

Python mysql example: arduino mysql

```python
import serial
import mysql.connector

cnx = mysql.connector.connect(
	user='root', password='80',
	host='127.0.0.1',
	database='product_database')

cursor = cnx.cursor()

query = ("SELECT name, quantity FROM products WHERE id = 2")
cursor.execute(query)
data = cursor.fetchone()

ser = serial.Serial("COM3", 9600)

# Wait for the arduino to setup
# the serial communication.
connected = False
while not connected:
	serin = ser.read()
	connected = True

ser.write(str(data[1]).encode())	# Send the product quantity to arduino
print(ser.read().decode())	# Wait for the response.
ser.write(b'10') # Send the max quantity to arduino
print(ser.read().decode())

ser.close()	# Close serial communication.
cursor.close() # Close cursor
cnx.close()	# Close database connection

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
