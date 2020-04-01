---
title: socket example 2 this do the job again (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example '2 this do the job again'


Modules used in program: 
* `import time`
* `import socket`

## python 2 this do the job again

Python socket example: 2 this do the job again

```python
import socket
import time

s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

s.sendto(b'first', ('192.168.1.1', 5556))
time.sleep(0.5)
s.sendto(b'second', ('192.168.1.1', 5556))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
