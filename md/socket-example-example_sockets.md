---
title: socket example example sockets (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'example sockets'

Functions in program: 
* `def run():`

Modules used in program: 
* `import socket`

## python example sockets

Python socket example: example sockets

```python
import socket
from common import URL, check

def run():
    protocol, rest = URL.split('://', 1)
    host, path = rest.split('/', 1)
    path = '/%s' % path
    s = socket.socket()
    s.connect((host, 80))
    s.send('GET %s HTTP/1.1\r\nHost: %s\r\n\r\n' % (path, host))
    data = ''
    while True:
        block = s.recv(1024)
        data += block
        if not block:
            break
    s.close()
    raw_headers, content = data.split('\r\n\r\n')
    raw_status, raw_headers = raw_headers.split('\r\n', 1)
    status = raw_status[len('HTTP/1.1 '):]
    status_code = int(status.split(' ', 1)[0])
    headers = dict([[x.strip() for x in line.split(':', 1)] for line in raw_headers.split('\r\n') if line.strip()])
    return status_code, headers, content


if __name__ == '__main__':
    check(*run())


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
