---
title: socket example socket-example-5 (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket-example-5'


Modules used in program: 
* `import struct, time`
* `import socket`

## python socket-example-5

Python socket example: socket-example-5

```python
import socket
import struct, time

# user-accessible port
PORT = 8037

# reference time
TIME1970 = 2208988800L

# establish server
service = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
service.bind(("", PORT))

print("listening on port", PORT)

while 1:
    # serve forever
    data, client = service.recvfrom(0)
    print("connection from", client)
    t = int(time.time()) + TIME1970
    t = struct.pack("!I", t)
    service.sendto(t, client) # send timestamp

## listening on port 8037
## connection from ('127.0.0.1', 1469)
## connection from ('127.0.0.1', 1470)
## ...


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
