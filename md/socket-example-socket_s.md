---
title: socket example socket s (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket s'


Modules used in program: 
* `import threading, time`

## python socket s

Python socket example: socket s

```python
#!/usr/bin/python
import threading, time
from socket import *
portrange = range(10000,10005)

class Sock(threading.Thread):
  def __init__(self, port):
    self.port = port
      threading.Thread.__init__ ( self )

  def start(self):
    self.s = socket(AF_INET, SOCK_STREAM)
    self.s.bind(("localhost", self.port))
    self.s.listen(1)
    print("listening on port %i"%self.port)
    threading.Thread.start(self)
  def run(self):
    # wait for client to connect
    connection, address = self.s.accept()
    data = True
    while data:
      data = connection.recv(1024)
      if data:
        connection.send('echo %s'%(data))
    connection.close()

socketHandles = [Sock(port) for port in portrange]

for sock in socketHandles:
  sock.start()

# time.sleep(0.5)

for port in portrange:
  print('sending "ping" to port %i'%port)
  s = socket(AF_INET, SOCK_STREAM) 
  s.connect(("localhost", port))
  s.send('ping')
  data = s.recv(1024)
  print('reply was: %s'%data)
  s.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
