---
title: socket example socket demo (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket demo'

Functions in program: 
* `def echo_client_compatiable(host, port):`
* `def echo_server_compatiable(host, port):`
* `def echo_client(host, port):`
* `def echo_server(host, port):`

Modules used in program: 
* `import sys`
* `import socket`

## python socket demo

Python socket example: socket demo

```python
#!/usr/bin/env python3

import socket
import sys

HOST = ''
PORT = 9000

def echo_server(host, port):
    '''
        TCP/IPv4 echo server
        A server must perform the sequence : socket(), bind(), listen(), accept();
        a server socket is for listening; 
        and new socket returned from accpet() used to sendall() /recv();
    '''
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.bind((host, port))
        s.listen(1)
        conn, addr = s.accept()
        with conn:
            print('Connected by', addr)
            while True:
                data = conn.recv(1024)
                if not data:
                    break
                conn.sendall(data)
                
                
def echo_client(host, port):
    '''
        TCP/IPv4 echo client
        A client only needs the sequence: socket(), connect();
        sendall()/recv() on the socket obj; 
    '''
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.connect((host, port))
        s.sendall(b'Hello, world')
        data = s.recv(1024)
    print('Received', repr(data))
    
 
def echo_server_compatiable(host, port):
    s = None
    for res in socket.getaddrinfo(host, port, socket.AF_UNSPEC,
                                  socket.SOCK_STREAM, 0, socket.AI_PASSIVE):
        af, socktype, proto, canonname, sa = res
        try:
            s = socket.socket(af, socktype, proto)
        except OSError as msg:
            s = None
            continue
        try:
            s.bind(sa)
            s.listen(1)
        except OSError as msg:
            s.close()
            s = None
            continue
        break
    if s is None:
        print('could not open socket')
        sys.exit(1)
    conn, addr = s.accept()
    with conn:
        print('Connected by', addr)
        while True:
            data = conn.recv(1024)
            if not data:
                break
            conn.send(data)
            
            
def echo_client_compatiable(host, port):
    s = None
    for res in socket.getaddrinfo(host, port, socket.AF_UNSPEC, socket.SOCK_STREAM):
        af, socktype, proto, cannonname, sa = res
        try:
            s = socket.socket(af, socktype, proto)
        except OSError as msg:
            s = None
            continue
        try:
            s.connect(sa)
        except OSError as msg:
            s.close()
            s = None
            continue
        break
    if s is None:
        print('could not open socket')
        sys.exit(1)
    with s:
        s.sendall(b'Hello, world')
        data = s.recv(1024)
    print('Received', repr(data))
    
if __name__ == '__main__':
    echo_server()
    # echo_client()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
