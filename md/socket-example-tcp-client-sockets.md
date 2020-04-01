---
title: socket example tcp-client-sockets (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'tcp-client-sockets'


Modules used in program: 
* `import socket`

## python tcp-client-sockets

Python socket example: tcp-client-sockets

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import socket

target_host = "127.0.0.1"
target_port = 80

# create a socket object
client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# connect the client
client.connect((target_host, target_port))

# send some data
client.send("input some data here")

# receive some data
response = client.recv(4096)

# print(response)
print(response)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
