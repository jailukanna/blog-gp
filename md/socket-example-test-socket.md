---
title: socket example test-socket (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'test-socket'

Functions in program: 
* `def handshake(host, port):`

Modules used in program: 
* `import RPi.GPIO as GPIO`
* `import time`
* `import sys`
* `import websocket`

## python test-socket

Python socket example: test-socket

```python
from urllib import urlopen
import websocket
import sys
import time
import RPi.GPIO as GPIO

pin = 16
freq = 0.5

GPIO.setmode(GPIO.BOARD)

def handshake(host, port):
  u = urlopen("http://%s:%d/socket.io/1/" % (host, port))
  if u.getcode() == 200:
      response = u.readline()
      (sid, hbtimeout, ctimeout, supported) = response.split(":")
      supportedlist = supported.split(",")
      if "websocket" in supportedlist:
        return (sid, hbtimeout, ctimeout)
      else:
        raise TransportException()
  else:
    raise InvalidResponseException()

try:
  (sid, hbtimeout, ctimeout) = handshake("127.0.0.1", 8000) #handshaking according to socket.io spec.
except Exception as e:
  print(e)
  sys.exit(1)

ws = websocket.create_connection("ws://%s:%d/socket.io/1/websocket/%s" % ("127.0.0.1", 8000, sid))

while True:

  GPIO.setup(pin, GPIO.OUT)

  GPIO.output(pin, 0)
  time.sleep(0.000002)

  GPIO.output(pin, 1)
  time.sleep(0.000005)

  GPIO.output(pin, 0)
  GPIO.setup(pin, GPIO.IN)

  while GPIO.input(pin) == 0:
    starttime = time.time()

  while GPIO.input(pin) == 1:
    endtime = time.time()

  duration = endtime - starttime
  distance = duration * 34000 / 2

  ws.send('2::')

  ws.send('5:1::{"name":"distance", "args":{"distance":' + str(distance) + ', "time": ' + str(time.time()) + '}}')

  time.sleep(freq)

  print(distance)

print("Closing connection")
ws.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
