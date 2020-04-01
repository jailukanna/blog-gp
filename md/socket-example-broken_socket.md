---
title: socket example broken socket (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'broken socket'


## python broken socket

Python socket example: broken socket

```python
from circuits import Manager, Debugger; from circuits.core.pollers import Select; from circuits.net.sockets import TCPServer, TCPClient; from circuits.net.events import close, connect, write
m = Manager() + Select() + Debugger()
tcp_server = TCPServer(0, secure=True, certfile='/home/git/circuits/tests/net/cert.pem', channel='server')
tcp_client = TCPClient(channel='client')
tcp_server.register(m)
tcp_client.register(m)
m.start()
from time import sleep; sleep(1)
tcp_client.fire(connect(*tcp_server._sock.getsockname(), **{'secure':True}))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
