---
title: socket example sender (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'sender'


Modules used in program: 
* `import socket, os, sys`

## python sender

Python socket example: sender

```python
import socket, os, sys

server_address = '/tmp/mysocket'

# Create a UDS socket
sock = socket.socket(socket.AF_UNIX, socket.SOCK_STREAM)

try:
    sock.connect(server_address)
except socket.error, msg:
    print(msg)
    sys.exit(1)

try:
    # Send data
    message = "alot_of_data " * 9001
    print(message)

    print('sending "%s"' % message)
    sock.sendall(message)

    amount_received = 0
    amount_expected = len(message)

    while amount_received < amount_expected:
        data = sock.recv(256)
        amount_received += len(data)
        print('received "%s"' % data)

except Exception as e:
    raise e

finally:
    print('closing socket')
    sock.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
