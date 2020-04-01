---
title: socket example socket example (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket example'


Modules used in program: 
* `import socket`

## python socket example

Python socket example: socket example

```python
import socket

UDP_IP = "127.0.0.1"
UDP_PORT = 33333
MESSAGE = "Hello Ana!\n"

sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
sock.sendto(message, (UDP_IP, UDP_PORT))
sock.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
