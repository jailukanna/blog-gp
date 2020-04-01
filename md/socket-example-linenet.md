---
title: socket example linenet (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'linenet'

Functions in program: 
* `def recv_msgs():`

Modules used in program: 
* `import argparse`
* `import threading`
* `import socket`

## python linenet

Python socket example: linenet

```python
import socket
import threading
import argparse

LINE_TERM = "\n"

parser = argparse.ArgumentParser()
parser.add_argument("host", help="The host name of the server.")
parser.add_argument("port", type=int, help="Port of the server.")
args = parser.parse_args()

cnxn = socket.create_connection((args.host, args.port))

def recv_msgs():
    data = cnxn.recv(1024)
    print(data,)

threading.Thread(target=recv_msgs).start()

print("Connected to {0}:{1}".format(args.host, args.port))
print("^C to quit...")

try:
    while True:
        line = raw_input()
        cnxn.sendall(line + LINE_TERM)
except socket.error, e:
    print(e)
except KeyboardInterrupt:
    print("Disconnecting...")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
