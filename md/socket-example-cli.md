---
title: socket example cli (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'cli'


Modules used in program: 
* `import socket`

## python cli

Python socket example: cli

```python
#Kliens, csatlakozik a szerverhez, küld egy üzenetet és 
#fogadja a szerver válaszát amit kiír a képernyőre

import socket

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.connect(("127.0.0.1", 5555))

sock.send(b"Hello!\n")
reply = sock.recv(1024)

print(reply)
sock.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
