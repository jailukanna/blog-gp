---
title: socket example client ssl (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'client ssl'


Modules used in program: 
* `import ssl`
* `import socket`

## python client ssl

Python socket example: client ssl

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
import ssl

host_addr = '127.0.0.1'
host_port = 443
server_sni_hostname = 'example.com'
server_cert = 'server.pem'
client_cert = 'client.pem'
client_key = 'client.key'

context = ssl.create_default_context(ssl.Purpose.SERVER_AUTH, cafile=server_cert)
context.load_cert_chain(certfile=client_cert, keyfile=client_key)

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
conn = context.wrap_socket(s, server_side=False, server_hostname=server_sni_hostname)
conn.connect((host_addr, host_port))
print("SSL established. Peer: {}".format(conn.getpeercert()))
print("Sending: 'Hello, world!")
conn.send(b"Hello, world!")
print("Closing connection")
conn.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
