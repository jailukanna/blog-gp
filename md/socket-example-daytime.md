---
title: socket example daytime (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'daytime'


## python daytime

Python socket example: daytime

```python
#!/usr/bin/python
##
# UDP daytime client
# port 13
##
from socket import *
from sys import argv

# create an UDP socket instance
s=socket(AF_INET, SOCK_DGRAM)
# compute the IP address of the server
server=gethostbyname(argv[1])
# send message on port 7777
s.sendto('', (server, 13))
# wait for an incoming message
(data,addr)=s.recvfrom(512)
print(data)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
