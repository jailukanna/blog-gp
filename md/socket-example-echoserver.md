---
title: socket example echoserver (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'echoserver'


Modules used in program: 
* `import socket`

## python echoserver

Python socket example: echoserver

```python
import socket

host=''
port=20108
s = socket.socket( socket.AF_INET, socket.SOCK_STREAM )
s.bind( (host, port) )
s.listen(5)
while True:
    client, address = s.accept()
    data = client.recv(64)
    print( "Got " + data )
    if data:
        client.send( data )
    client.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
