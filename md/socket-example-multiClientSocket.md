---
title: socket example multiClientSocket (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'multiClientSocket'

Functions in program: 
* `def handle(connAddr, j):`
* `def updateVal():`

Modules used in program: 
* `import json`
* `import time`
* `import threading  # Handle Threads`
* `import socket  # Import socket module`

## python multiClientSocket

Python socket example: multiClientSocket

```python
import socket  # Import socket module
import threading  # Handle Threads
import time
import json

lock = threading.Lock()
i = [0]

def updateVal():
    while True:
        i[0] = i[0]+1
        time.sleep(2)

def handle(connAddr, j):
    print("New connection")
    conn = connAddr[0]
    while True:
        time.sleep(2)
        with lock:
            conn.send(str(j[0]).encode())


s = socket.socket()         # Create a socket object
host = socket.gethostname() # Get local machine name
port = 50000                # Reserve a port for your service.

print(('Server started!'))
print(('Waiting for clients...'))

try:
    s.bind((host, port))        # Bind to the port
    s.listen(5)                 # Now wait for client connection.
except Exception as e:
    print(e)

# Thread that will update the var i
threading.Thread(target=updateVal, args=()).start()

while True:
    t = threading.Thread(target=handle, args=(s.accept(),i))
    t.start()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
