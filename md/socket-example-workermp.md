---
title: socket example workermp (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'workermp'

Functions in program: 
* `def worker(server_address):`

Modules used in program: 
* `import socket`
* `import os`

## python workermp

Python socket example: workermp

```python
# workermp.py

from multiprocessing.connection import Client
from multiprocessing.reduction import recv_handle
import os
import socket

def worker(server_address):
    serv = Client(server_address, authkey=b'peekaboo')
    serv.send(os.getpid())
    while True:
        fd = recv_handle(serv)
        print('WORKER: GOT FD', fd)
        with socket.socket(socket.AF_INET, socket.SOCK_STREAM, fileno=fd) as client:
            while True:
                msg = client.recv(1024)
                if not msg:
                    break
                print('WORKER: RECV {!r}'.format(msg))
                client.send(msg)
    
if __name__ == '__main__':
    import sys
    if len(sys.argv) != 2:
        print('Usage: worker.py server_address', file=sys.stderr)
        raise SystemExit(1)

    worker(sys.argv[1])


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
