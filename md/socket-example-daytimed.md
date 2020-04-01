---
title: socket example daytimed (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'daytimed'


## python daytimed

Python socket example: daytimed

```python
#!/usr/bin/python
##
# UDP daytime server
# port 13
##
from socket import *
from datetime import datetime

# create an UDP socket instance
s=socket(AF_INET, SOCK_DGRAM)
# associate the socket to local port 13
s.bind(('0.0.0.0', 13))
print("Listening on port 13")
while True:
  # wait for an incoming message
  (data,addr)=s.recvfrom(512)
  print("incoming request from " + str(addr))
  # send datetime
  s.sendto(datetime.now().strftime('%c\n'),addr)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
