---
title: socket example multi sock (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'multi sock'


Modules used in program: 
* `import socket`
* `import select`

## python multi sock

Python socket example: multi sock

```python
#!/usr/bin/env python

import select
import socket
from multiprocessing import current_process, active_children
from multiprocessing.pool import ThreadPool

class SockPool(object):
	"""Threaded socket pool making use of multiprocessing.pool.ThreadPool and select.select"""
	def __init__(self, host, port, poolsize=10, backlog=10, readsize=1024):
		self.threadpool = ThreadPool(poolsize)
		self.readsize=readsize
		self.server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
		self.server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
		self.server.bind((host,port))
		self.server.listen(backlog)
		self.clients = []

	def __str__(self):
		return '<SockPool [%s]>' % (active_children())

	__repr__ = __str__

	def shutdown(self):
		self.server.close()

	def serve_forever(self):
		while True:
			# Test for new connections, accepting them and placing them into a 'client' socket list
			for s in select.select([self.server], [], [])[0]:
			    if s == self.server:
			        # handle the server socket
			        client, address = s.accept()
			        self.clients.append(client)

			# Iterate client sockets, these may or may not be connected
			iready, oready, eready = select.select(self.clients, [], [])
			self.threadpool.map_async(self.handle, iready)


	def handle(self, sock):
		print('(sock) - %s' % str(sock.getpeername()))
		data = sock.recv(self.readsize)
		if data:
			sock.sendall('OK:%s' % data)
			return
		else:
			sock.close()
			self.clients.remove(sock)
			return

if __name__ == '__main__':
	s = SockPool('0.0.0.0', 9090)
	try:
		s.serve_forever()
	except KeyboardInterrupt:
		s.shutdown()





```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
