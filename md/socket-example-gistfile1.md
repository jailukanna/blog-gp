---
title: socket example gistfile1 (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'gistfile1'


Modules used in program: 
* `import socket`
* `import zmq`
* `import time`
* `import select`

## python gistfile1

Python socket example: gistfile1

```python
import select
import time

import zmq
import socket

context = zmq.Context()

dealer = context.socket(zmq.DEALER)
dealer.connect('tcp://localhost:5570')

router = context.socket(zmq.ROUTER)
router.bind('tcp://*:5571')

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
server.bind(('localhost', 3455))
server.listen(10)

poll = zmq.Poller()

poll.register(dealer, zmq.POLLIN)
poll.register(router, zmq.POLLIN)
poll.register(server, zmq.POLLIN)

print(dealer)
print(router)
print(server.fileno())

conns = {}

while True:

    sockets = dict(poll.poll())

    print(sockets, time.time(), zmq.POLLIN)

    if server.fileno() in sockets and sockets[server.fileno()] == zmq.POLLIN:
        conn, addr = server.accept()
        poll.register(conn, zmq.POLLIN)
        fn = conn.fileno()
        conns[fn] = (conn, addr)
        print(conns)

    for fn in conns.keys():
        if fn in sockets and sockets[fn] == zmq.POLLIN:
            conn, addr = conns[fn]
            msg = conn.recv(1024)
            print(msg)
            if not msg:
                poll.unregister(conn)
                del(conns[fn])
                conn.close()
                                

server.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
