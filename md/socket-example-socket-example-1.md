---
title: socket example socket-example-1 (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket-example-1'


Modules used in program: 
* `import struct, time`
* `import socket`

## python socket-example-1

Python socket example: socket-example-1

```python
import socket
import struct, time

# server
HOST = "www.python.org"
PORT = 37

# reference time (in seconds since 1900-01-01 00:00:00)
TIME1970 = 2208988800L # 1970-01-01 00:00:00

# connect to server
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((HOST, PORT))

# read 4 bytes, and convert to time value
t = s.recv(4)
t = struct.unpack("!I", t)[0]
t = int(t - TIME1970)

s.close()

# print(results)
print("server time is", time.ctime(t))
print("local clock is", int(time.time()) - t, "seconds off")

## server time is Sat Oct 09 16:42:36 1999
## local clock is 8 seconds off


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
