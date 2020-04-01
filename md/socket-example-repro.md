---
title: socket example repro (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'repro'

Functions in program: 
* `def two():`
* `def one():`

Modules used in program: 
* `import socket`
* `import threading`
* `import time`

## python repro

Python socket example: repro

```python
from kombu import Connection

import time
import threading
import socket


AMQP_URI = "pyamqp://guest:guest@localhost:5672/"
DEFAULT_TIMEOUT = 1.0

run = threading.Event()

socket.setdefaulttimeout(DEFAULT_TIMEOUT)


def one():
    while run.is_set():
        with Connection(AMQP_URI) as conn:
            conn.collect(socket_timeout=0.1)


def two():
    while run.is_set():
        try:
            sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            assert sock.gettimeout() == DEFAULT_TIMEOUT
        except AssertionError:
            run.clear()
            raise


run.set()

t1 = threading.Thread(target=one)
t2 = threading.Thread(target=two)

try:
    t1.start()
    t2.start()
    while run.is_set():
        time.sleep(1)
except KeyboardInterrupt:
    run.clear()
    t1.join()
    t2.join()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
