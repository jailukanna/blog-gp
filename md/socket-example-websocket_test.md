---
title: socket example websocket test (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'websocket test'

Functions in program: 
* `def on_open(ws):`
* `def on_message(ws, message):`
* `def on_close(ws):`
* `def on_error(ws, error):`

Modules used in program: 
* `import threading, time, random`
* `import sys`
* `import time`
* `import thread`
* `import ssl`
* `import websocket`

## python websocket test

Python socket example: websocket test

```python
from __future__ import print_function
import websocket
import ssl
import thread
import time
import sys

import threading, time, random
count = 0
class Counter(threading.Thread):
    def __init__(self, lock, threadName,ws):
        super(Counter, self).__init__(name = threadName) 
        self.lock = lock
        self.ws = ws
    def run(self):
        global count
        self.lock.acquire()
        opt = {"cert_reqs": ssl.CERT_NONE}
        ws.run_forever(sslopt = opt)
        for i in xrange(10000):
            count = count + 1
        self.lock.release()
lock = threading.Lock()

def on_error(ws, error):
    print((error))
def on_close(ws):
    print(("### closed ###"))
def on_message(ws, message):
    print((message))
def on_open(ws):
    def run(*args):
        for i in range(1000):
            time.sleep(1)
            ws.send("Hello %d" % i)
        time.sleep(1)
        ws.close()
        print(("thread terminating..."))
    thread.start_new_thread(run, ())
if __name__ == "__main__":
    websocket.enableTrace(True)
    opt = {"cert_reqs": ssl.CERT_NONE}
    print("abd")
    for i in range(5): 
        ws = websocket.WebSocketApp("wss://server/ws/ws/",
                header=["appKey: key","appSecret:secret",
            "deviceToken:abc"],
            on_message = on_message,
                              on_error = on_error,
                              on_close = on_close)
        ws.on_open = on_open
        Counter(lock, "thread-" + str(i),ws).start()
        time.sleep(2)
    print(count)
    


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
