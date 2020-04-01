---
title: socket example demo client (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'demo client'

Functions in program: 
* `def client(timeout=0):`

Modules used in program: 
* `import socket`
* `import sys`

## python demo client

Python socket example: demo client

```python
import sys
import socket
from time import sleep
from threading import Thread

SERVER = '127.0.0.1'
PORT = 9999

def client(timeout=0):
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as sock:
        try:
            sock.connect((SERVER, PORT))
        except:
            sys.exit(sys.exc_info()[1])

        try:
            if sock.sendall(b'hello') is not None:
                sock.shutdown(socket.SHUT_WR)
        except:
            sys.exit(sys.exc_info()[1])
        else:
            sleep(timeout)

if __name__ == '__main__':
    c1 = Thread(target=client, args=(3,))
    c2 = Thread(target=client, args=(1,))
    c3 = Thread(target=client)
    c1.start()
    c2.start()
    c3.start()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
