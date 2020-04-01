---
title: socket example echod (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'echod'


## python echod

Python socket example: echod

```python
#!/usr/bin/python
##
# UDP echo server
# port 7777
##
from socket import *

# create an UDP socket instance
s=socket(AF_INET, SOCK_DGRAM)
# associate the socket to local port 7777
s.bind(('0.0.0.0', 7777))
print("Listening on port 7777")
while True:
  # wait for an incoming message
  (data,addr)=s.recvfrom(512)
  print("%s: %d %s" % (addr, len(data), repr(data)))
  # send it back!
  s.sendto(data,addr)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
