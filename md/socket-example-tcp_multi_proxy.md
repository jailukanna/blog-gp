---
title: socket example tcp multi proxy (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'tcp multi proxy'


Modules used in program: 
* `import sys`
* `import socket`
* `import select`

## python tcp multi proxy

Python socket example: tcp multi proxy

```python
import select
import socket
import sys

socks = []
sockmap = {}

srv = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
srv.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
srv.bind(('0.0.0.0', 5555))
srv.listen()
socks.append(srv)

while True:
    readable, _, _ = select.select(socks, [], [], 0)
    for s in readable:
        if s == srv:
            cli, _ = srv.accept()
            remote = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            remote.connect((sys.argv[1], int(sys.argv[2])))
            sockmap[cli] = remote
            sockmap[remote] = cli
            socks.append(cli)
            socks.append(remote)
        else:
            msg = s.recv(4096)
            if not msg:
                s.close()
                sockmap[s].close()
                socks.remove(s)
                socks.remove(sockmap[s])
            else:
                sockmap[s].sendall(msg)

srv.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
