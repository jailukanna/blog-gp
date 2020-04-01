---
title: socket example multi (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'multi'


Modules used in program: 
* `import select`
* `import socket`

## python multi

Python socket example: multi

```python
#Chat broadcast szerver: fogadja a kiensek csatlakozását és az üzeneteiket
#továbbküldi a csatlakozott klienseknek

import socket
import select

socks = []

srv = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
srv.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
srv.bind(("0.0.0.0", 5555))
srv.listen()
socks.append(srv)

while True:
    readable, writeable, err = select.select(socks, [], [], 0)

    for s in readable:
        if s == srv:
            client, client_address = srv.accept()
            print("New client from", client_address)
            socks.append(client)
        else:
            msg = s.recv(1024)
            if msg: #[sock.send(msg) for sock in socks if sock != s and sock != srv]
                for sock in socks:
                    if sock != s and sock != srv:
                        sock.send(msg)
            else:
                socks.remove(s)
                s.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
