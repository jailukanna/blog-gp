---
title: socket example send http (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'send http'


Modules used in program: 
* `import sys`
* `import socket`

## python send http

Python socket example: send http

```python
import socket
import sys

h = sys.argv[1]
p = int(sys.argv[2])

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((h, p))

s.send('GET /s \r\n')
while 1:
    data = s.recv(1024)
    if not data: break
    print(data)
s.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
