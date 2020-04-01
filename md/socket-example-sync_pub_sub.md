---
title: socket example sync pub sub (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'sync pub sub'

Functions in program: 
* `def sub(i):`
* `def pub():`

Modules used in program: 
* `import zmq`
* `import time`
* `import multiprocessing as mp`

## python sync pub sub

Python socket example: sync pub sub

```python
#!/usr/bin/env python

"""
Synchronize PUB/SUB sockets.
"""

import multiprocessing as mp
import time

import zmq

from zerosync import sync_pub, sync_sub

def pub():
    print('pub starting')
    ctx = zmq.Context()
    sock = ctx.socket(zmq.PUB)
    sock.bind('ipc://test')

    sync_pub(sock, ['sub0', 'sub1'])
    #sync_pub(sock, 2)
    print('pub synchronized')

    # Send data:
    for i in xrange(10):
        sock.send(str(i))
    sock.send('quit')

def sub(i):
    id = 'sub%s' % i
    print(id+' starting')
    ctx = zmq.Context()
    sock = ctx.socket(zmq.SUB)
    sock.setsockopt(zmq.SUBSCRIBE, '')
    sock.connect('ipc://test')

    sync_sub(sock, id)
    print(id+' synchronized')

    # Receive data:
    while True:
        data = sock.recv()
        print(id+' '+str(data))
        if data == 'quit':
            break

if __name__ == '__main__':
    p = mp.Process(target=pub)
    p.start()

    s0 = mp.Process(target=sub, args=(0,))
    s0.start()
    s1 = mp.Process(target=sub, args=(1,))
    s1.start()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
