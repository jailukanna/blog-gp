---
title: socket example socket reuseport (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket reuseport'


Modules used in program: 
* `import os`
* `import socket`

## python socket reuseport

Python socket example: socket reuseport

```python
"""test with echo data | nc localhost 10000
"""
import socket
import os

SO_REUSEPORT = 15

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
#s.setsockopt(socket.SOL_SOCKET,socket.SO_REUSEADDR,1)
s.setsockopt(socket.SOL_SOCKET, SO_REUSEPORT, 1)
s.bind(('', 10000))
s.listen(1)
while True:
    conn, addr = s.accept()
    print('Connected to {}'.format(os.getpid()))
    data = conn.recv(1024)
    conn.send(data)
    conn.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
