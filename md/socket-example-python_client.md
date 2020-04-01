---
title: socket example python client (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'python client'


Modules used in program: 
* `import sys`
* `import socket`

## python python client

Python socket example: python client

```python
# Taken from Python MOTW

import socket
import sys

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

server = ('localhost', 5678)
s.connect(server)
print('Connecting to {} at port {}', *server)

try:

	message = b'This is a message. This will be repeated'
	print('Sending message: {}', message)
	s.sendall(message)

	amount_received = 0
	amount_expected = len(message)

	while amount_received < amount_expected:
		data = s.recv(16)
		amount_received += len(data)
		print('received {!r}'.format(data))

finally:
	print('closing socket')
	s.close()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
