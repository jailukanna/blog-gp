---
title: socket example socket-example-2 (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket-example-2'


Modules used in program: 
* `import struct, time`
* `import socket`

## python socket-example-2

Python socket example: socket-example-2

```python
import socket
import struct, time

# user-accessible port
PORT = 8037

# reference time
TIME1970 = 2208988800L

# establish server
service = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
service.bind(("", PORT))
service.listen(1)

print("listening on port", PORT)

while 1:
    # serve forever
    channel, info = service.accept()
    print("connection from", info)
    t = int(time.time()) + TIME1970
    t = struct.pack("!I", t)
    channel.send(t) # send timestamp
    channel.close() # disconnect

## listening on port 8037
## connection from ('127.0.0.1', 1469)
## connection from ('127.0.0.1', 1470)
## ...


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
