---
title: socket example socket-rcv (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket-rcv'

Functions in program: 
* `def main():`

Modules used in program: 
* `import socket`

## python socket-rcv

Python socket example: socket-rcv

```python
from __future__ import print_function
import socket
from contextlib import closing

def main():
    host = ''
    port = 20000
    bufsize = 2048 # more than 1518?

    sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    with closing(sock):
        sock.bind((host, port))
        while True:
            print(sock.recv(bufsize))
    return

if __name__ == '__main__':
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
