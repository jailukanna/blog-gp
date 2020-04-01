---
title: socket example udp sender (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'udp sender'


Modules used in program: 
* `import socket`

## python udp sender

Python socket example: udp sender

```python
import socket
 
UDP_IP = "127.0.0.1"
UDP_PORT = 5005
 
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM) # UDP
sock.bind((UDP_IP, 0))
sock.sendto(b"abc", (UDP_IP, UDP_PORT))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
