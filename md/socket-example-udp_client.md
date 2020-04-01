---
title: socket example udp client (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'udp client'


Modules used in program: 
* `import socket`

## python udp client

Python socket example: udp client

```python
import socket
from datetime import datetime

server_address = ('localhost', 6789)
max_size = 4096

print('Starting the client at', datetime.now())
client = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
client.sendto(b'Hey!', server_address)
data, server = client.recvfrom(max_size)
print('At', datetime.now(), server, 'said', data)
client.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
