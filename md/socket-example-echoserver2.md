---
title: socket example echoserver2 (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'echoserver2'

Functions in program: 
* `def handle(c, c_addr):`

Modules used in program: 
* `import threading`
* `import socket`

## python echoserver2

Python socket example: echoserver2

```python
#!/usr/bin/env python3
# Simple little echo server for python 3.

import socket
import threading

def handle(c, c_addr):
    f = c.makefile('rwb')
    while True:
        line = f.readline()
        if not line: break
        print("Received from connection:", line)
        f.write(line)
        f.flush()
    f.close()
    c.close()

if __name__ == '__main__':
    s = socket.socket()
    s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    s.bind(('0.0.0.0', 5999))
    s.listen(5)
    while True:
        (c, c_addr) = s.accept()
        threading.Thread(target = lambda: handle(c, c_addr)).start()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
