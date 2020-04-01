---
title: socket example socket info (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket info'

Functions in program: 
* `def info_about_socket(s, out=stdout):`

Modules used in program: 
* `import socket`

## python socket info

Python socket example: socket info

```python
import socket
from subprocess import check_output
from sys import stdout
from os import getpid

def info_about_socket(s, out=stdout):
    """
    Write some information about the status and state of a socket object
    to the file-like object 'out' (stdout, by default).

    This has come in handy in the past when comparing the sockets in two
    different processes to see why they were acting differently.
    """

    def info(msg):
        out.write(msg + '\n')

    info("fd for %s is %d" % (s, s.fileno()))
    info(check_output("lsof -p %d | awk '$4~/^%d/'" % (getpid(), s.fileno()), shell=True))

    t = s.gettimeout()
    if t is None:
        info("%d socket is blocking" % s.fileno())
    else:
        info("%d socket is nonblocking: timeout is %s" % (s.fileno(), t))

    for name, opt, buflen in (
        ('SO_ACCEPTCONN', socket.SO_ACCEPTCONN, 0),
        ('SO_BINDTODEVICE', 25, 100),
        ('SO_BROADCAST', socket.SO_BROADCAST, 0),
        ('SO_BSDCOMPAT', 14, 0),
        ('SO_DEBUG', socket.SO_DEBUG, 0),
        ('SO_DOMAIN', 39, 0),
        ('SO_ERROR', socket.SO_ERROR, 0),
        ('SO_DONTROUTE', socket.SO_DONTROUTE, 0),
        ('SO_KEEPALIVE', socket.SO_KEEPALIVE, 0),
        ('SO_LINGER', socket.SO_LINGER, 32),
        ('SO_OOBINLINE', socket.SO_OOBINLINE, 0),
        ('SO_PASSCRED', 16, 0),
        ('SO_PEERCRED', 17, 100),
        ('SO_PRIORITY', 12, 0),
        ('SO_PROTOCOL', 38, 0),
        ('SO_RCVBUF', socket.SO_RCVBUF, 0),
        ('SO_RCVTIMEO', socket.SO_RCVTIMEO, 50),
        ('SO_SNDTIMEO', socket.SO_SNDTIMEO, 50),
        ('SO_REUSEADDR', socket.SO_REUSEADDR, 0),
        ('SO_SNDBUF', socket.SO_SNDBUF, 0),
        ('SO_RCVLOWAT', socket.SO_RCVLOWAT, 0),
        ('SO_SNDLOWAT', socket.SO_SNDLOWAT, 0),
        ('SO_TIMESTAMP', 29, 0),
        ('SO_TYPE', socket.SO_TYPE, 0),
    ):
        try:
            val = s.getsockopt(socket.SOL_SOCKET, opt, buflen)
        except socket.error, e:
            info("%s: (%s)" % (name, e))
        else:
            info("%s: %r" % (name, val))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
