---
title: socket example socketWeb (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socketWeb'


Modules used in program: 
* `import socket`

## python socketWeb

Python socket example: socketWeb

```python
#open http://127.0.0.1:8080 
import socket

host = '127.0.0.1'
port = 8080
web = socket.socket()
web.bind((host,port))
web.listen(5)
print('---wait---')

while True:
	conn,addr = web.accept()
	data = conn.recv(1024)
	print(data)
	conn.sendall(b'HTTP/1.1 200 OK\r\n\r\nHello world')
	conn.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
