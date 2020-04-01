---
title: socket example bind socket (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'bind socket'


Modules used in program: 
* `import sys	#for exit`
* `import socket	#for sockets`

## python bind socket

Python socket example: bind socket

```python
#!/usr/bin/python

import socket	#for sockets
import sys	#for exit

try:
	#create an AF_INET, STREAM socket (TCP)
	s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
except socket.error, msg:
	print('Failed to create socket. Error code: ' + str(msg[0]) + ' , Error message : ' + msg[1])
	sys.exit();

print('Socket Created')

host = 'www.google.com'
port = 80

try:
	remote_ip = socket.gethostbyname( host )

except socket.gaierror:
	#could not resolve
	print('Hostname could not be resolved. Exiting')
	sys.exit()
	
print('Ip address of ' + host + ' is ' + remote_ip)

#Connect to remote server
s.connect((remote_ip , port))

print('Socket Connected to ' + host + ' on ip ' + remote_ip)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
