---
title: socket example sockets sample (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'sockets sample'


Modules used in program: 
* `import socket`
* `import socket`

## python sockets sample

Python socket example: sockets sample

```python
#server.py
import socket
serversocket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
serversocket.bind(('localhost', 8089))
serversocket.listen(5) #Max 5 connections.
while True:
    connection, address = serversocket.accept()
    buf = connection.recv(64)
    if len(buf) > 0:
        print(buf.decode('utf-8')) #Receive bytes obj.
        break
        
        
#client.py
import socket
clientsocket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
clientsocket.connect(('localhost', 8089))
clientsocket.send('hello'.encode('utf-8')) #Send bytes obj.

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
