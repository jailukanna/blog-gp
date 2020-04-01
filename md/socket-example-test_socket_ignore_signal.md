---
title: socket example test socket ignore signal (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'test socket ignore signal'

Functions in program: 
* `def readline(host, port):`
* `def sprint(*args):`
* `def sigalrm_handler(signum, frame):`

Modules used in program: 
* `import time`
* `import sys`
* `import socket`
* `import signal`
* `import os`

## python test socket ignore signal

Python socket example: test socket ignore signal

```python
#!/usr/bin/env python
"""Test whether socket ignores signals."""
import os
import signal
import socket
import sys
import time
from subprocess import Popen


class Alarm(Exception):
    pass


def sigalrm_handler(signum, frame):
    raise Alarm


def sprint(*args):
    print((("client",) + args))  # print(tuple to support Python 2.4+)


def readline(host, port):
    """Read a single line from host:port.

    From  http://docs.python.org/release/3.3.0/library/socket.html
    """
    s = None
    for _ in range(10):
        for res in socket.getaddrinfo(host, port,
                                      socket.AF_UNSPEC, socket.SOCK_STREAM):
            af, socktype, proto, canonname, sa = res
            sprint(sa)
            try:
                s = socket.socket(af, socktype, proto)
            except socket.error:
                s = None
                continue
            try:
                s.connect(sa)
            except socket.error:
                s.close()
                s = None
                continue
            break
        else:
            # no break; server might not be ready yet
            if server_process.poll() is not None:
                break
            time.sleep(1)  # allow server to start
            continue
        break

    if s is None:
        sprint('could not open socket')
        sys.exit(1)
    s.sendall('Hello, world'.encode('ascii'))
    f = s.makefile('rb')
    try:
        data = f.readline()  # should block
    finally:
        f.close()
    s.close()
    sprint('Received', repr(data))
    return data

port = 12345
if len(sys.argv) > 1:
    host = sys.argv[1]
else:
    # get public network interface (according to docs)
    host = socket.gethostbyname(socket.gethostname())

# start server that doesn't respond
scriptdir = os.path.dirname(os.path.abspath(__file__))
server_script = os.path.join(scriptdir, 'server.py')
server_process = Popen([sys.executable, server_script, "%s:%s" % (host, port)])


# setup signal handler & alarm
signal.signal(signal.SIGALRM, sigalrm_handler)
delay = 3  # seconds
signal.alarm(delay)
sprint("wait %d seconds" % (delay,))
try:
    readline(host, port)
except Alarm:  # got alarm
    sprint('ALARM WORKS')
else:
    assert 0, "shouldn't get here, readline() must block"


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
