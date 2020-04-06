---
title: mysql example pi ble scan (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'pi ble scan'

Functions in program: 
* `def doQuery( conn, sql ) :`
* `def signal_handler(signal, frame):`

Modules used in program: 
* `import sys`
* `import signal`
* `import mysql.connector`

## python pi ble scan

Python mysql example: pi ble scan

```python
import mysql.connector
import signal
import sys
from bluepy.btle import Scanner, DefaultDelegate

hostname = '192.168.10.12'
username = 'yourname'
password = 'password'
database = 'new_schema'

myConnection = mysql.connector.connect( host=hostname, user=username, passwd=password, db=database )

def signal_handler(signal, frame):
    print('You pressed Ctrl+C!')
    myConnection.close()
    sys.exit(0)


def doQuery( conn, sql ) :
    cur = conn.cursor()
    cur.execute(sql)
    myConnection.commit()


class ScanDelegate(DefaultDelegate):
    def __init__(self):
        DefaultDelegate.__init__(self)

    def handleDiscovery(self, dev, isNewDev, isNewData):
        if isNewDev:
            print("Discovered device", dev.addr)
        elif isNewData:
            print("Received new data from", dev.addr)


signal.signal(signal.SIGINT, signal_handler)
scanner = Scanner().withDelegate(ScanDelegate())
myNumber = "01"

while True :
    devices = scanner.scan(5.0)
    for dev in devices:
        print("Device %s (%s), RSSI=%d dB" % (dev.addr, dev.addrType, dev.rssi))
	for (adtype, desc, value) in dev.getScanData():
            print("  %s = %s" % (desc, value))
	sqlStr = "INSERT INTO ble_devices (iotNumber, deviceUUID, rssi) VALUES ('%s', '%s', '%d')" % (myNumber, dev.addr, dev.rssi)
	doQuery( myConnection, sqlStr )

    print("==============================")



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
