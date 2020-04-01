---
title: socket example sockCheck (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'sockCheck'

Functions in program: 
* `def isOpen(ip,port):`

Modules used in program: 
* `import socket`

## python sockCheck

Python socket example: sockCheck

```python
import socket
def isOpen(ip,port):
   s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
   try:
      s.settimeout(1)
      s.connect((ip, int(port)))
      s.settimeout(None)
      s.shutdown(2)
      return True
   except:
      return False

print("{} is {}".format('192.168.1.55',isOpen('192.168.1.55',5666)))
print("{} is {}".format('192.168.1.65',isOpen('192.168.1.65',5666)))
~

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
