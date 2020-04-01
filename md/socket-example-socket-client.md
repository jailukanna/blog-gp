---
title: socket example socket-client (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket-client'


Modules used in program: 
* `import socket`

## python socket-client

Python socket example: socket-client

```python
#client example
import socket
client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
print('Connecting...')
client_socket.connect(('127.0.0.1', 8887))
print('Connected')
while 1:
    data = client_socket.recv(512)
    if ( data == 'q' or data == 'Q'):
        client_socket.close()
        break;
    else:
        print("RECEIVED:" , data)
        data = raw_input ( "SEND( TYPE q or Q to Quit):" )
        if (data <> 'Q' and data <> 'q'):
            client_socket.send(data)
        else:
            client_socket.send(data)
            client_socket.close()
            break;

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
