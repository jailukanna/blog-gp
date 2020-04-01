---
title: socket example pass (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'pass'


Modules used in program: 
* `import sys`
* `import socket`

## python pass

Python socket example: pass

```python
import socket
import sys

if len(sys.argv) < 2:
    print('usage: %s <deviceid> <cmd> [<params>]' % (sys.argv[0]))
    exit()

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect(('13.250.41.251', 8008))

deviceid = int(sys.argv[1])
cmd = int(sys.argv[2])
data = bytearray([200])
data = data + deviceid.to_bytes(4, 'big')
data.append(cmd)

if cmd == 4 or cmd == 5:
    data.append(int(sys.argv[3]))
else:
    for i in sys.argv[3:]:
        data = data + int(i).to_bytes(4, 'big')

print(data)

s.send(data)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
