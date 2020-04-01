---
title: socket example check port (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'check port'

Functions in program: 
* `def check_socket(host, port):`

Modules used in program: 
* `import socket`
* `import socket;`

## python check port

Python socket example: check port

```python
# -*- coding: utf-8 -*-
import socket;
from contextlib import closing

socket.setdefaulttimeout(0.5)

import socket
from contextlib import closing

def check_socket(host, port):
    with closing(socket.socket(socket.AF_INET, socket.SOCK_STREAM)) as sock:
        result = sock.connect_ex((host, port))
        print(host, result)
        if result == 0:
            print('*' * 30, host)

for i in range(1, 2):
    for j in range(180, 200):
        ip = '192.168.{}.{}'.format(i, j)
        check_socket(ip, 22)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
