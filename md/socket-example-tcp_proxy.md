---
title: socket example tcp proxy (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'tcp proxy'


Modules used in program: 
* `import sys`
* `import select`
* `import socket`

## python tcp proxy

Python socket example: tcp proxy

```python
import socket
import select
import sys

#python3 tcp_proxy.py 80 ik.elte.hu 80

srv = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
srv.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
srv.bind(("0.0.0.0", int(sys.argv[1])))
srv.listen()
client, client_address = srv.accept()

remote = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
remote.connect((sys.argv[2], int(sys.argv[3])))

while True:
    readable, _, _ = select.select((client, remote), [], [], 0)
    for s in readable:
        msg = s.recv(4096)
        if msg:
            if s == client:
                print("from client: *************")
                remote.send(msg)
            else:
                print("from remote: *************")
                client.send(msg)

            print(msg)

client.close()
remote.close()
srv.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
