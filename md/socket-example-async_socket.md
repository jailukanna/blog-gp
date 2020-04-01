---
title: socket example async socket (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'async socket'

Functions in program: 
* `def echo(conn):`
* `def run():`
* `def asyn(f):`
* `def _loop_():`

Modules used in program: 
* `import socket`

## python async socket

Python socket example: async socket

```python
import socket
from concurrent.futures import ThreadPoolExecutor, as_completed
from collections import deque
from functools import wraps

_addr_ = ('', 50007)
_queue_ = deque()
executor = ThreadPoolExecutor(max_workers=10)

def _loop_():
    while True:
        (_handle, fut) = _queue_.popleft()
        if not fut.done():
            _queue_.append((_handle, fut))
            continue
        try:
            _res = fut.result()
            _queue_.append((_handle, executor.submit(_handle.send, _res)))
        except:
            pass

def asyn(f):
    @wraps(f)
    def wrapper(*args, **kwds):
        handle = f(*args, **kwds)
        _queue_.append((handle, executor.submit(next, handle)))
        return handle
    return wrapper

@asyn
def run():
    while True:
        if len(_queue_) >= MAX_CONN:
            yield False
        print("waiting for connecting")
        (conn, addr) = yield s.accept()
        print('connected')
        echo(conn)

@asyn
def echo(conn):
    with conn:
        while True:
            print("waiting for input:")
            data = yield conn.recv(1024)
            print("recieved:", data)
            data = b'echo:'+data
            yield conn.send(data)

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:

    s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    s.bind(_addr_)
    s.listen(1)
    
    run()
    _loop_()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
