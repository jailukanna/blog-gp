---
title: socket example 1 but this doesnt (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example '1 but this doesnt'


Modules used in program: 
* `import time`
* `import socket`

## python 1 but this doesnt

Python socket example: 1 but this doesnt

```python
import socket
import time

s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

s.bind(('', 5556))
s.connect(('192.168.1.1', 5556))

s.send(b'first')
time.sleep(0.5)
s.send(b'second')  # ConnectionRefusedError


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
