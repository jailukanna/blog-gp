---
title: socket example chat-server (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'chat-server'


Modules used in program: 
* `import socket`
* `import socket`

## python chat-server

Python socket example: chat-server

```python
import socket

HOST = "127.0.0.1"
PORT = 50000

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((HOST, PORT))

s.send("This is a message from the client")

s.close()
JONATHANs-MBP:chat jcohen66$ cat server.py
import socket

HOST = ''
PORT = 50000
QUEUE_DEPTH = 1
RECV_BUFFER_SIZE = 1024

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.bind((HOST, PORT))     #Note this is a tuple
sock.listen(QUEUE_DEPTH)
conn, addr = sock.accept()

msg = conn.recv(RECV_BUFFER_SIZE)
print("Recv'd: %s" % (msg))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
