---
title: socket example abs srv (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'abs srv'


Modules used in program: 
* `import os`
* `import socket`

## python abs srv

Python socket example: abs srv

```python
# -*- coding: utf-8 -*-
import socket
import os

print("Opening socket...")
server = socket.socket(socket.AF_UNIX, socket.SOCK_STREAM)
server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
print("socket.create")
server.bind(b"\0/var/tmp/sock.tmp")
print("socket.bind")
server.listen(10)
print("socket.listen")
print("Listening...")
connection, client_address = server.accept()
print("socket.accept")

while True:
    datagram = connection.recv(1024)
    if not datagram:
        break
    else:
        print("-" * 20)
        print(datagram.decode('utf-8'))
        if "DONE" == datagram.decode('utf-8'):
            break
print("-" * 20)
print("Shutting down...")
connection.close()
server.close()
print("Done")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
