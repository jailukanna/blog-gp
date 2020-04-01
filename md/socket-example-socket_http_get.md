---
title: socket example socket http get (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket http get'


Modules used in program: 
* `import socket`
* `import sys`

## python socket http get

Python socket example: socket http get

```python
#!/usr/bin/python

""" Socket script to test an HTTP GET. Freely modified from http://effbot.org/zone/socket-intro.htm"""

import sys
import socket

# Let the user choose and hostname to connect to
hostname = str(raw_input("Insert hostname: "))

# Create the socket
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Connect to http port
s.connect((hostname, 80))

# Send HTTP GET
s.send("GET / HTTP/1.0\r\n\r\n")

# Load received stuff in a 1000 char buffer and cicle output to stdout
while True:
    buf = s.recv(1000)
    if not buf:
        break
    sys.stdout.write(buf)

s.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
