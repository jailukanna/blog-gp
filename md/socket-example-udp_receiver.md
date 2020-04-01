---
title: socket example udp receiver (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'udp receiver'


Modules used in program: 
* `import socket`

## python udp receiver

Python socket example: udp receiver

```python
import socket
 
UDP_IP = "127.0.0.1"
UDP_PORT = 5005
 
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM) # UDP
sock.bind((UDP_IP, UDP_PORT))


print(sock.setsockopt(socket.SOL_SOCKET, socket.SO_RCVBUF, 0))

while True:
    data, addr = sock.recvfrom(1024) # buffer size is 1024 bytes
    print(data)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
