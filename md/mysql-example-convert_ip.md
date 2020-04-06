---
title: mysql example convert ip (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'convert ip'

Functions in program: 
* `def string2int(s):`
* `def format_ip_string(i):`
* `def extract(cidr):`

Modules used in program: 
* `import sys, struct, socket`
* `import mysql.connector`

## python convert ip

Python mysql example: convert ip

```python
import mysql.connector
import sys, struct, socket

def extract(cidr):
    (ip, cidr) = cidr.split('/')
    cidr = int(cidr)
    host_bits = 32 - cidr
    i = struct.unpack('>I', socket.inet_aton(ip))[0]
    host_min = (i >> host_bits) << host_bits
    host_max = i | ((1 << host_bits) - 1)

    return (format_ip(host_min), format_ip(host_max))

def format_ip_string(i):
    return socket.inet_ntoa(struct.pack('>I',i))

def string2int(s):
    return reduce(lambda a,b: a<<8 | b, map(int, s.split(".")))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
