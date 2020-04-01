---
title: socket example chat client (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'chat client'


Modules used in program: 
* `import sys`
* `import select`
* `import socket`

## python chat client

Python socket example: chat client

```python
import socket
import select
import sys
 
server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

IP_address = '127.0.0.1'
Port = 14600
server.connect((IP_address, Port))
 
while True:
 
    
    sockets_list = [sys.stdin, server]
 
    
    read_sockets,write_socket, error_socket = select.select(sockets_list,[],[])
 
    for socks in read_sockets:
        if socks == server:
            message = socks.recv(2048)
            print(message)
        else:
            message = sys.stdin.readline()
            server.send(message)
            sys.stdout.write("<You>")
            sys.stdout.write(message)
            sys.stdout.flush()
server.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
