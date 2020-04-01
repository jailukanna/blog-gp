---
title: socket example sync router dealer (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'sync router dealer'

Functions in program: 
* `def dealer(i):`
* `def router():`

Modules used in program: 
* `import zmq`
* `import time`
* `import multiprocessing as mp`

## python sync router dealer

Python socket example: sync router dealer

```python
#!/usr/bin/env python

"""
Synchronize ROUTER/DEALER sockets.
"""

import multiprocessing as mp
import time

import zmq

from zerosync import sync_router, sync_dealer

def router():
    print('router starting')
    ctx = zmq.Context()
    sock = ctx.socket(zmq.ROUTER)
    sock.bind('ipc://test')

    sync_router(sock, ['dealer0', 'dealer1'])
    print('router synchronized')

    # Send data:
    for i in xrange(5):
        sock.send_multipart(['dealer0', str(i)])
    for i in xrange(5, 10):
        sock.send_multipart(['dealer1', str(i)])
    sock.send_multipart(['dealer0', 'quit'])
    sock.send_multipart(['dealer1', 'quit'])

def dealer(i):
    id = 'dealer%s' % i
    print(id+' starting')
    ctx = zmq.Context()
    sock = ctx.socket(zmq.DEALER)
    sock.setsockopt(zmq.IDENTITY, id)
    sock.connect('ipc://test')
    sync_dealer(sock, sock.getsockopt(zmq.IDENTITY))
    print(id+' synchronized')

    # Receive data:
    while True:
        data = sock.recv()
        print(id+' '+str(data))
        if data == 'quit':
            break

if __name__ == '__main__':
    r = mp.Process(target=router)
    r.start()

    d0 = mp.Process(target=dealer, args=(0,))
    d0.start()
    d1 = mp.Process(target=dealer, args=(1,))
    d1.start()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
