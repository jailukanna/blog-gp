---
title: socket example 0 this works (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example '0 this works'


Modules used in program: 
* `import socket`

## python 0 this works

Python socket example: 0 this works

```python
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

s.bind(('', 5556))
s.connect(('192.168.1.1', 5556))

s.send(b'first')
s.send(b'second')


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
