---
title: socket example chat-client (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'chat-client'


Modules used in program: 
* `import socket`

## python chat-client

Python socket example: chat-client

```python
import socket

HOST = "127.0.0.1"
PORT = 50000

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((HOST, PORT))

s.send("This is a message from the client")

s.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
