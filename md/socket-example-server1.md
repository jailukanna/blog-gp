---
title: socket example server1 (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'server1'


Modules used in program: 
* `import socket`

## python server1

Python socket example: server1

```python
#coding: utf8

import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind(('127.0.0.1', 1024))
s.listen(10)

conn, addr = s.accept()

r = conn.recv(1024)
print(r)

# s.send('hi client, this is greeting from server')

conn.close()
s.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
