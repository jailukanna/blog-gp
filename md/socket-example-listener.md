---
title: socket example listener (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'listener'


Modules used in program: 
* `import socket, sys`

## python listener

Python socket example: listener

```python
#!/usr/bin/env python
import socket, sys

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
port = 6675
server_address = ("0.0.0.0", port)
sock.bind(server_address)
sock.listen(1)

while True:
   print("~ Waiting For Connection on port:", port)
   connection, client_address = sock.accept()
   try:
      print("~ Connection From:", client_address[0])
      while True:
         data = connection.recv(16)
         print("%s" % data)
         if data:
            print("~ Recieved Data")
            connection.sendall("1")
         else:
            print("~ No More Data From:", client_address[0])
            break
   finally:
      connection.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
