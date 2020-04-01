---
title: socket example udpSocket (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'udpSocket'


Modules used in program: 
* `import socket`

## python udpSocket

Python socket example: udpSocket

```python
import socket
target_host = "127.0.0.1"
target_port = 80
# create a socket object 
client = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
# send some data 
client.sendto("AAABBBCCC",(target_host,target_port))
# receive some data 
data, addr = client.recvfrom(4096)
print(data )

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
