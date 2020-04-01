---
title: socket example tcpSocket (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'tcpSocket'


Modules used in program: 
* `import socket`

## python tcpSocket

Python socket example: tcpSocket

```python
import socket
target_host = "www.google.com"
target_port = 80
# create a socket object 
client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# connect the client 
client.connect((target_host,target_port))
# send some data 
client.send("GET / HTTP/1.1\r\nHost: google.com\r\n\r\n")
# receive some data 
response = client.recv(4096)
print(response )

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
