---
title: socket example struct cli (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'struct cli'


Modules used in program: 
* `import struct`
* `import socket`

## python struct cli

Python socket example: struct cli

```python
import socket
import struct

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.connect(("127.0.0.1", 5555))

msg = (2019, b"HELLO", 0.5)
msg_packed = struct.Struct("I 5s f").pack(*msg)

sock.sendall(msg_packed)

sock.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
