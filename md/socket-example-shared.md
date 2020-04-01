---
title: socket example shared (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'shared'

Functions in program: 
* `def sendto(self, data, addr):`
* `def recvfrom(self, buflen):`
* `def print(*args, **kwargs):`

Modules used in program: 
* `import time`
* `import threading`
* `import sys`
* `import string`
* `import socket`
* `import builtins`

## python shared

Python socket example: shared

```python
import builtins
import socket
import string
import sys
import threading
import time

# These probably should have better names, or better yet, be function parameters.
IP = 'localhost'
PORT = int(sys.argv[1])

BUFFER_SIZE = 20

# Why isn't this a bytes object in the first place?
# All you do is decode, compare, and encode it.
CONNECT = 'CONNECT'
# Also, "CONNECT" seems like a rather misleading name for a UDP message,
# given that there is no such thing as a UDP connection.
# Unless you're on Windows and using SO_CONDITIONAL_ACCEPT, 

# Extra globals for logging
procname = sys.argv[2]
if len(sys.argv) >= 4:
    num_clients = int(sys.argv[3])
    def name_client(i):
        if i > 26:
            return '[Client {}]'.format(i)
        return '[Client {}]'.format(string.ascii_lowercase[i])
# I override a few functions to improve logging

# Print now includes time, process name, and thread name
def print(*args, **kwargs):
    thread = threading.current_thread()
    if threading.main_thread() is thread:
        threadname = ''
    else:
        threadname = ' ' + thread.name
    t = int(time.time() * 1e9)
    heading = '{} {}{}: '.format(t, procname, threadname)
    builtins.print(heading, *args, **kwargs, flush=True)

# Some socket methods now print(information)
socket.socket.REAL_RECVFROM = socket.socket.recvfrom
def recvfrom(self, buflen):
    data, addr = self.REAL_RECVFROM(buflen)
    print("socket got {} from {}".format(data, addr))
    return data, addr
socket.socket.recvfrom = recvfrom

socket.socket.REAL_SENDTO = socket.socket.sendto
def sendto(self, data, addr):
    result = self.REAL_SENDTO(data, addr)
    print("socket sent {} to {}".format(data, addr))
    return result
socket.socket.sendto = sendto


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
