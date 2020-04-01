---
title: socket example cliente (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'cliente'


Modules used in program: 
* `import socket`

## python cliente

Python socket example: cliente

```python
import socket

UDP_IP = "192.168.250.79"
UDP_PORT = 5005
MESSAGE = "Hola guapo"

print("UDP target IP:", UDP_IP)
print("UDP target port:", UDP_PORT)
print("message:", MESSAGE)

sock = socket.socket(socket.AF_INET, # Internet
                     socket.SOCK_DGRAM) # UDP
sock.sendto(MESSAGE, (UDP_IP, UDP_PORT))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
