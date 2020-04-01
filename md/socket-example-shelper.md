---
title: socket example shelper (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'shelper'


Modules used in program: 
* `import socket`

## python shelper

Python socket example: shelper

```python
import socket

class SocketHelper:

	s = None
	conn = None
	addr = None

	def __init__(self,host,port):
		self.s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
		self.s.bind((host, port))
		self.s.listen(5)

	def s_appept(self):
		self.conn,self.addr = self.s.accept()

	def send_data(self,content):
		self.conn.send(content)

	def read_data(self):
		buf = self.conn.recv(1024)
		return buf

	def close_socket(self):
		self.conn.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
