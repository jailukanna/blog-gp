---
title: socket example basic server (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'basic server'


Modules used in program: 
* `import socket`

## python basic server

Python socket example: basic server

```python
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM, 0)
s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
# REUSEPORT: 15
#s.setsockopt(socket.SOL_SOCKET, 15, 1)


s.bind(('0.0.0.0', 1234))
s.listen(10)

print(" [*] listening on :1234")
while True:
    (c, addr) = s.accept()
    print('got ', addr)
    c.send('hello world\n')
    c.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
