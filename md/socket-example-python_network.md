---
title: socket example python network (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'python network'


Modules used in program: 
* `import socket`

## python python network

Python socket example: python network

```python
# -*- coding:utf-8 -*-

import socket


# Server
HOST = '192.168.1.100'
PORT = 8001

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind((HOST, PORT))
s.listen(5) # 除当前正在处理的链接，最多允许有 5 个链接正在排队，再同时来的就直接拒绝链接

while True:
    conn, addr = s.accept()
    print('Connected by ', addr)
    
    while True:
        data = conn.recv(1024)
        print(data)
        
        conn.send("Server received your message.")
        
# conn.close() 
        
# Client
HOST = '192.168.1.100'
PORT = 8001

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((HOST, PORT))

while True:
    cmd = raw_input("Please input command:")
    s.send(cmd)
    data = s.recv(1024)
    print(data)
    
    # s.close()    

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
