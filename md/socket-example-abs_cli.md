---
title: socket example abs cli (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'abs cli'


Modules used in program: 
* `import os`
* `import socket`

## python abs cli

Python socket example: abs cli

```python
# -*- coding: utf-8 -*-
import socket
import os

print("Connecting...")
client = socket.socket(socket.AF_UNIX, socket.SOCK_STREAM)
client.connect(b"\0/var/tmp/sock.tmp")
print("Ready.")
print("Ctrl-C to quit.")
print("Sending 'DONE' shuts down the server and quits.")
while True:
    try:
        x = input("> ")
        if "" != x:
            print("SEND:", x)
            client.send(x.encode('utf-8'))
            if "DONE" == x:
                print("Shutting down.")
                break
    except KeyboardInterrupt as k:
        print("Shutting down.")
        client.close()
        break


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
