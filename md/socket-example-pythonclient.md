---
title: socket example pythonclient (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'pythonclient'


Modules used in program: 
* `import socket`

## python pythonclient

Python socket example: pythonclient

```python
import socket

client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect(("localhost", 6011))
while 1:
    data = raw_input ( "Enter text to be upper-cased, q to quit\n" )
    client_socket.send(data)
    if ( data == 'q' or data == 'Q'):
        client_socket.close()
        break;
    else:        
        data = client_socket.recv(5000)
        print("Your upper cased text:  " , data)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
