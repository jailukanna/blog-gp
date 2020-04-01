---
title: socket example struct srv (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'struct srv'


Modules used in program: 
* `import struct`
* `import socket`

## python struct srv

Python socket example: struct srv

```python
import socket
import struct

srv = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
srv.bind(("0.0.0.0", 5555))
srv.listen()
client, client_address = srv.accept()

msg_packed = client.recv(1024)
msg = struct.Struct("I 5s f").unpack(msg_packed)
print(msg)

client.close()
srv.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
