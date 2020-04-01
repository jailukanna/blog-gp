---
title: socket example client http (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'client http'


Modules used in program: 
* `import socket`

## python client http

Python socket example: client http

```python
# -*- coding: utf-8 -*-
import socket
from utils import readlines
"""
Un simple client HTTP.
"""

# Création de la socket TCP
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# On se connecte sur le port 80 de google
server_address = ('www.google.com', 80)
print('connecting to %s port %s' % server_address)
sock.connect(server_address)

try:
    # Send HTTP request
    request = input('> ')
    print('sending "%s"' % request)
    sock.sendall((request + '\n\n').encode())

    # Receive the response
    for line in readlines(sock, delim='\r\n'):
        print(line)
        if line == '\r':  # caractère de fin de réponse HTTP
            break
finally:
    # Close the connection
    sock.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
