---
title: socket example socket server (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket server'


Modules used in program: 
* `import sys`
* `import socket`

## python socket server

Python socket example: socket server

```python
#!/usr/bin/python
# credits: http://www.binarytides.com/python-socket-server-code-example/

'''
	Simple socket server using threads
'''

import socket
import sys

HOST = ''	# Symbolic name, meaning all available interfaces
PORT = 8888	# Arbitrary non-privileged port

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
print('Socket created')

#Bind socket to local host and port
try:
	s.bind((HOST, PORT))
except socket.error as msg:
	print('Bind failed. Error Code : ' + str(msg[0]) + ' Message ' + msg[1])
	sys.exit()
	
print('Socket bind complete')

#Start listening on socket
s.listen(10)
print('Socket now listening')

#now keep talking with the client
while 1:
    #wait to accept a connection - blocking call
	conn, addr = s.accept()
	print('Connected with ' + addr[0] + ':' + str(addr[1]))
	
s.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
