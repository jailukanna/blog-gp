---
title: socket example socket redis v2 (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket redis v2'


Modules used in program: 
* `import socket, time`

## python socket redis v2

Python socket example: socket redis v2

```python
#!/usr/bin/env python
import socket, time

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.connect(('localhost', 6379))
sock.setsockopt(socket.IPPROTO_TCP, socket.TCP_NODELAY, 1)

fp = sock.makefile('r')
sock.sendall('PING\r\n')
print(fp.readline()[:-2])
time.sleep(2)

fp = sock.makefile('r')
sock.sendall('EXISTS mylist\r\n')
print(fp.readline()[:-2])
time.sleep(2)

fp = sock.makefile('r')
sock.sendall('get for\r\n')
print(fp.read(int(fp.readline()[1:-2])))
time.sleep(2)

fp = sock.makefile('r')
sock.sendall('lrange mylist 0 -1\r\n')
n = int(fp.readline()[1:-2])
#print(n)
for i in range(n):
    response = fp.readline()[:-2][1:]
    l = 0 if len(response) == 0 else int(response)
    #print(l, '--', response)
    print(l and fp.read(l) or '')
    fp.read(2)

fp = sock.makefile('r')
sock.sendall('sort mylist\r\n')
n = int(fp.readline()[1:-2])
#print(n)
for i in range(n):
    response = fp.readline()[:-2][1:]
    l = 0 if len(response) == 0 else int(response)
    print(l and fp.read(l) or '')
    fp.read(2)

fp = None
sock.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
