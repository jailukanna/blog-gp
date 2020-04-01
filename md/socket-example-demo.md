---
title: socket example demo (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'demo'


Modules used in program: 
* `import time, socket`

## python demo

Python socket example: demo

```python
import time, socket

class ControllableSocket:
    def __init__(self, latency, bandwidth):
        self._latency = latency
        self._bandwidth = bandwidth
        self._bytesSent = 0
        self._timeCreated = time.time()
        self._socket = socket.socket()

    def send(self, bytes):
        now = time.time()
        connectionDuration = now - self._timeCreated
        self._bytesSent += len(bytes)
        # How long should it have taken to send how many bytes we've sent with our
        # given bandwidth limitation?
        requiredDuration = self._bytesSent / self._bandwidth
        time.sleep(max(requiredDuration - connectionDuration, self._latency))
        return self._socket.send(bytes)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
