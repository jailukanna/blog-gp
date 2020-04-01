---
title: socket example net client (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'net client'


Modules used in program: 
* `import socket`

## python net client

Python socket example: net client

```python
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
host = socket.gethostname()
port = 4560

# Connects to the server.
s.connect((host, port))
print('Connected.')

# Initiates the conversation.
s.send(bytes('PING', 'utf8'))
print('>', 'PING')

# Loop to listen for incoming data.
while True:

	try:
		data = s.recv(4096).decode('utf8')

		# Quits if it receives 'PONG'.
		if data == 'PONG':
			print('<', data)
			break

	except socket.error:
		print('Socket died.')
		raise

	except socket.timeout:
		print('Socket timeout.')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
