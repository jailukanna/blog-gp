---
title: socket example SocketServer (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'SocketServer'


Modules used in program: 
* `import socket`

## python SocketServer

Python socket example: SocketServer

```python
import socket

ADDRESS = '127.0.0.1'
PORT = 7777

with socket.socket() as s:
    s.bind((ADDRESS, PORT))
    s.listen(1)
    connectionObject, address = s.accept()
    connectionObject.sendall(b'Hello, World!')
    while True:
        with connectionObject:
            data = connectionObject.recv(1024)
            print(data)



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
