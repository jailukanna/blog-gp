---
title: socket example socket-example-4 (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket-example-4'


Modules used in program: 
* `import struct, time`
* `import socket`

## python socket-example-4

Python socket example: socket-example-4

```python
import socket
import struct, time

# server
HOST = "localhost"
PORT = 8037

# reference time (in seconds since 1900-01-01 00:00:00)
TIME1970 = 2208988800L # 1970-01-01 00:00:00

# connect to server
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

# send empty packet
s.sendto("", (HOST, PORT))

# read 4 bytes from server, and convert to time value
t, server = s.recvfrom(4)
t = struct.unpack("!I", t)[0]
t = int(t - TIME1970)

s.close()

print("server time is", time.ctime(t))
print("local clock is", int(time.time()) - t, "seconds off")

## server time is Sat Oct 09 16:42:36 1999
## local clock is 8 seconds off


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
