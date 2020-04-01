---
title: socket example file send (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'file send'

Functions in program: 
* `def open_bin(filename):`

Modules used in program: 
* `import socket`

## python file send

Python socket example: file send

```python
import socket

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.connect(("127.0.0.1", 5555))

def open_bin(filename):
    with open(filename, "rb") as f:
        while True:
            buff = f.read(1024)
            if not buff:
                return
            yield buff

prev = 0
md5 = hashlib.md5()
for buff in open_bin("data.bin"):
    prev = zlib.crc32(buff, prev)
    md5.update(buff)

    sock.send(buff)
else:
    print("CRC32: ", hex(prev))
    print("MD5: ", md5.hexdigest())


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
