---
title: socket example socket basic (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket basic'


Modules used in program: 
* `import socket`
* `import socket`

## python socket basic

Python socket example: socket basic

```python
'''hey guys we come back with socker programming with python
now if want to tell you this scripts are basics of socket module in python libraries
as you can see we use syntax to create server and client 
follow the commands
'''
#first we import socket for server.py
import socket
#now we create object for the server file to say that we use AF_INET(ipv4) and SOCK_STRAM(TCP stream)
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
#after that we bind it for say host and port from the client
s.bind((socket.gethostname(), 1234))
#listen the data
s.listen(5)
#make the while for send message
while True:
    clientsocket, address = s.accept()
    print('connection form {} is established'.format(address))
#now we choose the encode(UTF-8) to Encryption message
    clientsocket.send(bytes('Join to Py Spider Channel', 'utf-8'))
    


######################Client file#############################

'''client file is like the server file but we just make object to recive data from the server you know the client means that 
all of the time we reaciving data from the servers'''
#first we import socket for client.py

import socket
#now we create object for the client file to say that we use AF_INET(ipv4) and SOCK_STRAM(TCP stream)

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
#rather than the bind we just connect to reacive
client.connect((socket.gethostname(), 1234))
#now we reacive information from the server.py for 1024 buffer
message = client.recv(1024)
print(message)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
