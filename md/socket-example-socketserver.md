---
title: socket example socketserver (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socketserver'


## python socketserver

Python socket example: socketserver

```python
'''
Taken from Programming Python.
'''

from socket import *
myHost = ''
myPort = 50007

sockobj = socket(AF_INET, SOCK_STREAM)
sockobj.bind((myHost,myPort))
sockobj.listen(5)

while True:
    connection, address = sockobj.accept()
    print('Server connected by', address)
    while True:
        data = connection.recv(1024)
        if not data:
            break
        connection.send(b'Echo=>' + data)
    connection.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
