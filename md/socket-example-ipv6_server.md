---
title: socket example ipv6 server (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'ipv6 server'

Functions in program: 
* `def main():`
* `def listen_loop():`
* `def read_client(sock, mask):`
* `def read_server(sock, mask):`

Modules used in program: 
* `import time`
* `import sys`
* `import selectors`
* `import socket`

## python ipv6 server

Python socket example: ipv6 server

```python
import socket
import selectors
import sys
import time

HOST = None
# HOST = "localhost"  # "::1"  # "127.0.0.1", "None"
PORT = 2000
BACKLOG = 5
BUFSIZE = 1024
sel = selectors.DefaultSelector()


def read_server(sock, mask):
    try:
        conn, addr = sock.accept()
        print('Connected by', addr)
        sel.register(conn, selectors.EVENT_READ, read_client)
    except:
        pass
    try: conn.close()
    except: pass


def read_client(sock, mask):
    while True:
        data = sock.recv(BUFSIZE)
        if not data: break
        sock.send(data)
    sock.close()


def listen_loop():
    while True:
        while len(sel.get_map()) == 0:
            time.sleep(0.5)
        events = sel.select()
        for key, mask in events:
            callback = key.data
            callback(key.fileobj, mask)


def main():
    for res in socket.getaddrinfo(HOST, PORT, socket.AF_UNSPEC, socket.SOCK_STREAM, 0, socket.AI_PASSIVE):
        print(res)
        af, socktype, proto, canonname, sa = res
        try:
            sock = socket.socket(af, socktype, proto)
        except OSError as msg:
            print(1, msg)
            continue
        try:
            if af == socket.AF_INET:
                print("IPV4")
            elif af == socket.AF_INET6:
                print("IPV6")
            sock.bind(sa)
            sock.listen(BACKLOG)
            sel.register(sock, selectors.EVENT_READ, read_server)
        except OSError as msg:
            sock.close()
            print(2, msg)
            continue
 
    print(sel.fileno())
    if len(sel.get_map()) == 0:
        print('could not open socket')
        sys.exit(1)
    else:
        listen_loop()


if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
