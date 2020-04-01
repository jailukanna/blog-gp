---
title: socket example assignment server (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'assignment server'

Functions in program: 
* `def main(argv):`
* `def fatal_error(s):`

Modules used in program: 
* `import sys`

## python assignment server

Python socket example: assignment server

```python
"""Assignment 1: Server program
    Written by: Ryan McLaughlin
    Date: 21/8/2018
"""

import sys
from select import *
from socket import *
from datetime import *
from assignment_packet import *

class SocketGen:
    """Creates a Socket object and binds it to a port"""
    def __init__(self, port, L):
        self.port = int(port)
        self.language = L
        self.socket = socket(family = AF_INET, type = SOCK_DGRAM)
        self.socket.bind(('0.0.0.0', int(port)))
        #print("Socket opened on port {}".format(self.port))

    def __del__(self):
        """Deconstructor closes the socket so another process has access to it
        in the case the program terminates"""
        socket.close(self.socket)
        #print("Socket closed on port {}".format(int(self.port)))


def fatal_error(s):
    """Prints error code then terminates program"""
    print(s)
    exit(-1)

def main(argv):
    #input parameter checks:
    if len(argv) != 4:
        fatal_error("Server takes 3 parameters only")
    s = set()
    for port in argv[1:]:
        if port.isnumeric() == False:
            fatal_error("Port numbers must be integers")
        elif int(port) < 1024 or int(port) > 64000:
            fatal_error("Port numbers must be between 1024 and 64000")
        elif port in s:
            fatal_error("Port numbers must be unique")
        s.add(port)
    #Create socket object for each port
    sockets = [SocketGen(argv[i+1], i) for i in range(len(argv[1:]))]
    #enter main loop:
    while True:
        #asssociate sockets and their file descriptors:
        socket_fd = {s.socket.fileno() : s for s in sockets}
        #listen on sockets:
        rlist, _, _ = select(list(socket_fd),[],[])
        #for all readable sockets:
        for item in rlist:
            #Find the corresponding socket
            s = socket_fd[item]
            #read from it and unpack
            msg, addr = s.socket.recvfrom(4096)
            request_packet = DT_Request.unpack(msg, s.language + 1)
            #generate response packet and send to client
            response_packet = DT_Response(request_packet.request_type, request_packet.language, datetime.now() )
            if response_packet:
                s.socket.sendto(response_packet.pack(), addr)

if __name__ == '__main__':
    main(sys.argv)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
