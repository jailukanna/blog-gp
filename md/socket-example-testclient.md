---
title: socket example testclient (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'testclient'


Modules used in program: 
* `import logging`

## python testclient

Python socket example: testclient

```python
from socketIO_client import SocketIO, BaseNamespace
import logging

logging.basicConfig(level=logging.DEBUG)

class Namespace(BaseNamespace):

    def on_connect(self):
        print('[Connected]')

    def on_event(self, event, *args):
        print('Event:', event)

    def on_heartbeat(self):
        print('Heartbeat')

    def on_callme(self, callback):
        callback('Message for you, Sir.')

socketIO = SocketIO('localhost', 8080, Namespace)
piSpace = socketIO.define(Namespace, '/rpi')
piSpace.emit('test', 'bob')
socketIO.wait()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
