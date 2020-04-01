---
title: socket example socket-server (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket-server'


Modules used in program: 
* `import socket`

## python socket-server

Python socket example: socket-server

```python
#!/usr/bin/python
import socket
print('Starting server...')
s = socket.socket()
host = '127.0.0.1'
port = 8887
s.bind((host, port))
print('Bind')
s.listen(5)                 # Now wait for client connection.
while True:
   c, addr = s.accept()     # Establish connection with client.
   print('Got connection from', addr)
   c.send('Thank you for connecting')
   c.close()                # Close the connection

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
