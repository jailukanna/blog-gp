---
title: socket example udp-client-sockets (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'udp-client-sockets'


Modules used in program: 
* `import socket`

## python udp-client-sockets

Python socket example: udp-client-sockets

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import socket

target_host = "127.0.0.1"
target_port = 80

# create a socket object
client = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

# send some data
client.sendto("input some data here", (target_host, target_port))

# receive some data
(data, addr) = client.recvfrom(4096)

# print(response)
print(data)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
