---
title: socket example socketclient (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socketclient'


Modules used in program: 
* `import sys`

## python socketclient

Python socket example: socketclient

```python
'''
Taken from Programming Python
'''

import sys
from socket import *

serverHost = 'localhost'
serverPort = 500007
message = [b'Hello network world']
if len(sys.argv) > 1:
    serverHost = sys.argv[1]
    if len(sys.argv) > 2:
        message = (x.encode() for x in sys.argv[2:])

sockobj = socket(AF_INET, SOCK_STREAM)
sockobj.connect((serverHost,serverPort))

for line in message:
    sockobj.send(line)
    data = sockobj.recv(1024)
    print('Client received: ',data)
    
sockobj.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
