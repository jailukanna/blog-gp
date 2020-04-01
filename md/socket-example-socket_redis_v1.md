---
title: socket example socket redis v1 (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket redis v1'


Modules used in program: 
* `import socket, time`

## python socket redis v1

Python socket example: socket redis v1

```python
#!/usr/bin/env python
import socket, time

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.connect(('localhost', 6379))
sock.setsockopt(socket.IPPROTO_TCP, socket.TCP_NODELAY, 1)
time.sleep(2)
sock.sendall('PING\r\n')
print(repr(sock.recv(4096)))
time.sleep(2)
sock.sendall('get for\r\n')
print(repr(sock.recv(4096)))
time.sleep(2)
sock.sendall('lrange mylist 0 -1\r\n')
print(repr(sock.recv(4096)))
sock.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
