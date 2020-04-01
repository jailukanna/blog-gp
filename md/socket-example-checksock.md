---
title: socket example checksock (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'checksock'


Modules used in program: 
* `import socket, sys`

## python checksock

Python socket example: checksock

```python
from __future__ import print_function
import socket, sys

sock = socket.socket(socket.AF_UNIX, socket.SOCK_STREAM)
addr,path = sys.argv[1].split(":")
sock.connect(addr)

sock.send("""GET {} HTTP/1.1
host: test.ru

""".format(path))
print(sock.recv(1024))



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
