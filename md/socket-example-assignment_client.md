---
title: socket example assignment client (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'assignment client'

Functions in program: 
* `def main(argv):`
* `def fatal_error(s):`

Modules used in program: 
* `import sys`

## python assignment client

Python socket example: assignment client

```python
"""Assignment 1: Client program
    Written by: Ryan McLaughlin
    Date: 21/8/2018
"""

import sys
from select import *
from socket import *
from datetime import *
from assignment_packet import *

class SocketsError(Exception):
    def __init__(self, msg):
        self.msg = msg

class Socket:
    """Creates and closes a socket object"""
    def __init__(self, host, port):
        #create socket:
        self.socket = socket(family = AF_INET, type = SOCK_DGRAM)
        #check port
        try:
            self.addr = getaddrinfo(host, port, family = AF_INET)
        except gaierror:
            raise SocketsError("Hostname could not be resolved")
        #check socket initiaslised:
        if not self.socket:
            raise SocketsError("Socket could not be opened")
        #print("Socket opened")

    def __del__(self):
        #close socket:
        socket.close(self.socket)
        #print("Socket closed")

def fatal_error(s):
    """Prints error code then terminates program"""

    print(s)
    exit(-1)

def main(argv):
    #Checks command line inputs:
    if len(argv) != 4:
        fatal_error("Client takes 3 parameters only")

    req_type = str(argv[1])
    req_host = str(argv[2])
    req_port = int(argv[3])

    if req_type not in ['time', 'date']:
        fatal_error("First Parameter must be 'date' or 'time'")
    elif req_port < 1024 or req_port > 64000:
        fatal_error("Port parameter out of range (1024, 64000)")

    #Create DT_Request packet
    req_type = 1 if req_type == "time" else 2
    request_packet = DT_Request(req_type)
    message = request_packet.pack()
    #try to send:
    try:
        #create socket:
        s = Socket(req_host, req_port)
        #get address:
        addr = getaddrinfo(req_host, req_port, family = AF_INET)[0][-1]
        #send message:
        s.socket.sendto(message, addr)
        #wait for response:
        rlist, _, _ = select([s.socket],[],[], 1)
        #if a socket is readable
        if len(rlist):
            try:
                #try to read it
                package = s.socket.recv(4096)
            except ConnectionResetError:
                fatal_error("No response from socket")
        else:
            fatal_error("Recieve timed out")
    except SocketsError as e:
        fatal_error(e.msg)

    try:
        #try to unpack packet
        response = DT_Response.unpack(package)
    except PacketsError as e:
        fatal_error(e.msg)
    #print(the resulting string)
    print(response)


if __name__ == '__main__':
    #run program with command line inputs
    main(sys.argv)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
