---
title: socket example ip (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'ip'


Modules used in program: 
* `import socket`

## python ip

Python socket example: ip

```python
import socket

BUF_SIZE = 4096
HOST = 'httpbin.org'

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.connect((HOST, 80))

req_msg = [
    b'GET /ip HTTP/1.1',
    b'User-Agent: curl/7.37.1',
    'Host: {}'.format(HOST).encode('utf-8'),
    b'Accept: */*',
]
delimiter = b'\r\n'

sock.send(delimiter.join(req_msg))
sock.send(delimiter)
sock.send(delimiter)

data = b''
while True:
    http_response = sock.recv(BUF_SIZE)
    data += http_response
    if len(http_response) < BUF_SIZE:
        break

print(data.decode('utf-8'))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
