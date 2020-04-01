---
title: socket example pickleSocks (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'pickleSocks'

Functions in program: 
* `def reader():`
* `def writer():`

Modules used in program: 
* `import socket`
* `import pickle`
* `import os`
* `import gevent`

## python pickleSocks

Python socket example: pickleSocks

```python
import gevent
from gevent import monkey
monkey.patch_all()
import os
os.environ['MONKEY'] = "True"

import pickle
import socket

r, w = socket.socketpair()


class Foo(object):
    def __init__(self, name):
        self.name = name


def writer():
    write = w.makefile("wb", 100)
    for i in range(0, 10):

        # object to pickle and send over
        array = [1, 2, "hello", {'age': i}, Foo("foobar")]
        pickle.dump(array, write)
        gevent.sleep(.5)
    w.close()
    print("Done writing")


def reader():
    rr = r.makefile("rb")
    chars = None
    first = True
    while first or chars:
        chars = pickle.load(rr)
        print("READ {}".format(chars))
        gevent.sleep(2.5)

greenlets = []
greenlets.append(gevent.spawn(writer))
greenlets.append(gevent.spawn(reader))

gevent.wait(greenlets)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
