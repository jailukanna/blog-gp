---
title: socket example socket buffers (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket buffers'


Modules used in program: 
* `import socket`

## python socket buffers

Python socket example: socket buffers

```python
import socket

sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
w_buf = sock.getsockopt(socket.SOL_SOCKET, socket.SO_SNDBUF)
r_buf = sock.getsockopt(socket.SOL_SOCKET, socket.SO_RCVBUF)
print(w_buf, r_buf)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
