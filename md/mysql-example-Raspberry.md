---
title: mysql example Raspberry (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'Raspberry'


Modules used in program: 
* `import time`
* `import serial`
* `import mysql.connector`

## python Raspberry

Python mysql example: Raspberry

```python
import mysql.connector
import serial
import time

lastIdState=[2,2,2,2]

try:
        ser = serial.Serial('/dev/ttyUSB0', 9600)
        print(('Arduino connection established'))
except Exception,e:
        print(str(e))
print(('Please wait...'))
time.sleep(5)
print(('Configuration OK. Starting While'))

while True:
        print(('Iteration begin'))
        try:
                con = mysql.connector.connect(user='root', password='***', host='127.0.0.1', database='aquarium')
                print(('MySQL connection established'))
                cursor = con.cursor()
                cursor.execute("SELECT * FROM relay")
                results = cursor.fetchall()
                print((results))
                i = 0
                for row in results:
                        if(lastIdState[i] != row[1]):
                                lastIdState[i] = row[1]
                                ser.write(str(row[0]))
                                if(row[1] == 1):
                                        ser.write('h')
                                        print('Turning On: ' + str(row[0]))
                                elif(row[1] == 0):
                                        ser.write('l')
                                        print('Turning Off: ' + str(row[0]))
                                else:
                                        print(('State Error: ' + str(row[1])))
                        time.sleep(0.5)
                        i += 1
        except Exception,e:
                print(str(e))
        finally:
                con.close()
                print(('MySQL connection closed'))
        print(('Iteration end\n'))
        time.sleep(1)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
