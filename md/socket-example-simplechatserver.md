---
title: socket example simplechatserver (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'simplechatserver'


Modules used in program: 
* `import socket`

## python simplechatserver

Python socket example: simplechatserver

```python
import socket

host = ''
port = 12345

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind((host, port))
s.listen(5)
print('server is started port %s' % (port))
conn = None
while True:
	if conn is None:
		print('waiting for connection...')
		conn, addr = s.accept()
		print('got connection from', addr)
	else: 
		print('waiting for response...')
		print(conn.recv(1024))
		msg = raw_input('say something:...')
        	conn.sendall(msg)
        	if msg == 'exit()':
        		break
s.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
