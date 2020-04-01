---
title: socket example server ssl (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'server ssl'


Modules used in program: 
* `import ssl`
* `import socket`

## python server ssl

Python socket example: server ssl

```python
#!/usr/bin/python3

# Before run the code, generate the auto-signed certificate and the key
# 
# openssl req -new -x509 -days 365 -nodes -out client.pem -keyout client.key
#
# openssl req -new -x509 -days 365 -nodes -out server.pem -keyout server.key
#  
# Don't forget to set the parameters of certs, like hostname = 'example.com'

import socket
from socket import AF_INET, SOCK_STREAM, SO_REUSEADDR, SOL_SOCKET, SHUT_RDWR
import ssl

listen_addr = '127.0.0.1'
listen_port = 443
server_cert = 'server.pem'
server_key = 'server.key'
client_certs = 'client.pem'

context = ssl.create_default_context(ssl.Purpose.CLIENT_AUTH)
context.verify_mode = ssl.CERT_REQUIRED
context.load_cert_chain(certfile=server_cert, keyfile=server_key)
context.load_verify_locations(cafile=client_certs)

bindsocket = socket.socket()
bindsocket.bind((listen_addr, listen_port))
bindsocket.listen(5)

while True:
    print("Waiting for client")
    newsocket, fromaddr = bindsocket.accept()
    print("Client connected: {}:{}".format(fromaddr[0], fromaddr[1]))
    conn = context.wrap_socket(newsocket, server_side=True)
    print("SSL established. Peer: {}".format(conn.getpeercert()))
    buf = b''  # Buffer to hold received client data
    try:
        while True:
            data = conn.recv(4096)
            if data:
                # Client sent us data. Append to buffer
                buf += data
            else:
                # No more data from client. Show buffer and close connection.
                print("Received:", buf)
                break
    finally:
        print("Closing connection")
        conn.shutdown(socket.SHUT_RDWR)
        conn.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
