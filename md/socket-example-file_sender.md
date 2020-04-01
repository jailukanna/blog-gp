---
title: socket example file sender (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'file sender'


Modules used in program: 
* `import socket`

## python file sender

Python socket example: file sender

```python
import socket

client = socket.socket()

client.connect(("127.0.0.1", 9999))

with open('client_udp.py', 'rb') as f:
    for i in f:
        client.send(i)
        msg = client.recv(1024)
        if msg != b'success':
            break
    pass
client.send('quit'.encode())
client.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
