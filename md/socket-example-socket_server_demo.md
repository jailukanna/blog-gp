---
title: socket example socket server demo (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket server demo'

Functions in program: 
* `def test_udp_client():`
* `def test_tcp_client():`

Modules used in program: 
* `import socket`

## python socket server demo

Python socket example: socket server demo

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import socket
from socketserver import TCPServer
from socketserver import UDPServer
from socketserver import BaseRequestHandler


class MyTCPHandler(BaseRequestHandler):
    """
        tcp request handler.
    """
    def handle(self):
        # self.request is the TCP socket connected to the server
        data = self.request.recv(1024).strip()
        print("{} wrote:".format(self.client_address[0]))
        print(data)
        self.request.sendall(data.upper())


class MyUDPHandler(BaseRequestHandler):
    """
        udp request handler
    """

    def handle(self):
        # self.request consist of data and socket
        data = self.request[0].strip()
        socket = self.request[1]
        print("{} wrote: ".format(self.client_address[0]))
        print(data, self.client_address)
        socket.sendto('hello, world'.encode(), self.client_address)

def test_tcp_client():
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.connect(('', 9998))
    s.sendall('hello, world'.encode())
    resp = s.recv(1024)
    print('recived data: {}'.format(resp))


def test_udp_client():
    s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    # s.connect(('', 9997))
    s.sendto('hello, world'.encode(), ('', 9997))
    resp = s.recvfrom(1024)
    print('recived data: {}'.format(resp))

if __name__ == '__main__':
    HOST, PORT = 'localhost', 9997

    # create the tcp server, binding to local on port 9998
    # HOST, PORT = 'localhost', 9999
    # server = TCPServer((HOST, PORT), MyTCPHandler)

    # create the udp server, binding to 9997
    # HOST, PORT = 'localhost', 9997
    server = UDPServer((HOST, PORT), MyUDPHandler)

    # Activate the server; this will keep running until you
    # interrupt the program with the control + C
    server.serve_forever()
    

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
