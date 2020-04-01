---
title: socket example socket sample (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket sample'

Functions in program: 
* `def handle_connection(connection):`

Modules used in program: 
* `import sys`
* `import socket`

## python socket sample

Python socket example: socket sample

```python
# http://facebook.com/code.we.bas

import socket
import sys
from thread import *
 
# Define HOST and PORT
HOST = '' # Host, Can be Empty to work on all available interfaces. 
PORT = 8003 # Port to be used, must not be already used by another process.

# Create the Socket
our_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Try binding to HOST and PORT
try:
    our_socket.bind((HOST, PORT))
except socket.error as error:
    print('Error #' + str(error[0]) + ': ' + error[1])
    sys.exit()

# Begin Listenning
our_socket.listen(10)

def handle_connection(connection):
        connection.send('Welcome to our Server ..\n')
        
        while True:
			in_msg = connection.recv(1024)
			if not in_msg:
				break
			connection.send('You said: ' + in_msg)
					
        connection.close()
 
# Keep trying to Accept any new connections ..
while 1:                                                                                                                        
    connection, address = our_socket.accept()
    print('Connection from IP: ' + address[0] + ' on Port: ' + str(address[1]))
    start_new_thread(handle_connection, (connection,))
 
our_socket.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
