---
title: socket example py uds srv (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'py uds srv'

Functions in program: 
* `def main():`
* `def prep():`

Modules used in program: 
* `import base64`
* `import os`
* `import sys`
* `import socket`

## python py uds srv

Python socket example: py uds srv

```python
import socket
import sys
import os
import base64

def prep():
    srv_sock = './srv_socket'
    try:
        os.unlink(srv_sock)
    except:
        if os.path.exists(srv_sock):
            raise
    sock = socket.socket(socket.AF_UNIX, socket.SOCK_STREAM)
    try:
        print('Setting up server socket on %s\n' % srv_sock)
        sock.bind(srv_sock)
        sock.listen(1)
        print('Success, listening on unix domain socket %s\n' % srv_sock)
        return sock
    except Exception as e:
        print(e)
        return False

def main():
    sock = prep()
    if not sock:
        return False
    while True:
        connection, client_address = sock.accept()
        try:
            buffer = ''
            rcvd_bytes = 0
            while True:
                data = connection.recv(1024)
                rcvd_bytes = rcvd_bytes + len(data)
                if not data:
                    print('Finished receiving data\n')
                    break
                buffer = buffer + data
            buffer = buffer[:-1]
            print(base64.b64encode(buffer.rstrip('\x00')))
            print('Total bytes received bytes: %s\n' % len(buffer))
        finally:
            print('Closing the connection\n')
            connection.close()
            print('Connection closed\n')

if __name__=="__main__":
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
