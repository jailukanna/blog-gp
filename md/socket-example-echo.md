---
title: socket example echo (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'echo'


## python echo

Python socket example: echo

```python
#!/usr/bin/python
##
# UDP echo client
# port 7777
##
from socket import *
from sys import argv

# create an UDP socket instance
s=socket(AF_INET, SOCK_DGRAM)
msg="supercalifragilisticexpialidocious"
# compute the IP address of the server
server=gethostbyname(argv[1])
# send message on port 7777
s.sendto(msg, (server, 7777))
print("sent " + msg)
# wait for an incoming message
(data,addr)=s.recvfrom(512)
print("recv " + data)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
