---
title: socket example servidor (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'servidor'


Modules used in program: 
* `import socket`

## python servidor

Python socket example: servidor

```python
import socket

UDP_IP = "0.0.0.0"
UDP_PORT = 5005

sock = socket.socket(socket.AF_INET, # Internet
                     socket.SOCK_DGRAM) # UDP
sock.bind((UDP_IP, UDP_PORT))

while True:
    data, addr = sock.recvfrom(1024) # buffer size is 1024 bytes
    print("received message:", data)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
