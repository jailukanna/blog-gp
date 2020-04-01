---
title: socket example sockets-select (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'sockets-select'

Functions in program: 
* `def main():`
* `def event_loop():`
* `def receive(client_sock: socket):`
* `def acctpt(server_sock: socket):`

Modules used in program: 
* `import socket`

## python sockets-select

Python socket example: sockets-select

```python
import socket
from select import select

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  # args: IPv4, TCP/IP
server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)  # args: socket layer, reuse addr without timeout
server.bind(('localhost', 5000))
server.listen()

to_monitor = []


def acctpt(server_sock: socket):
    client, addr = server_sock.accept()
    print('connection from: ', addr)
    to_monitor.append(client)


def receive(client_sock: socket):
    request = client_sock.recv(4096)
    if request:
        response = 'server: I got msg "{}"\n'.format(request[:-1].decode())  # [:-1] - cut newline
        client_sock.send(response.encode())
    else:
        client_sock.close()
        to_monitor.remove(client_sock)



def event_loop():
    while True:
        ready_to_read, _, _ = select(to_monitor, [], [])
        for sock in ready_to_read:
            if sock is server:
                acctpt(sock)
            else:
                receive(sock)


def main():
    to_monitor.append(server)
    event_loop()


if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
