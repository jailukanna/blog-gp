---
title: socket example daytimedtcp (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'daytimedtcp'


## python daytimedtcp

Python socket example: daytimedtcp

```python
#!/usr/bin/python
##
# TCP daytime server
# port 13
##
from socket import *
from datetime import datetime

# create an UDP socket instance
s=socket(AF_INET, SOCK_STREAM)
# associate the socket to local port 13
s.bind(('0.0.0.0', 13))
s.listen(3)
print("Listening on port 13")
while True:
  # wait for an incoming connection
  (c,addr)=s.accept()
  print("incoming request from " + str(addr))
  # send datetime
  c.send(datetime.now().strftime('%c\n'))
  c.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
