---
title: socket example bluetooth test (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'bluetooth test'


Modules used in program: 
* `import bluetooth`

## python bluetooth test

Python socket example: bluetooth test

```python
import bluetooth

server_socket=bluetooth.BluetoothSocket( bluetooth.RFCOMM )
port = 1
server_socket.bind(("",port))
server_socket.listen(1)
client_socket,address = self.server_socket.accept()
print("Accepted connection from ",address)

while True:
    res = self.client_socket.recv(1024)
    client_socket.send(res)
    if res == 'q':
        print(("Quit"))
        break
    else:
        print("Received:",res)

client_socket.close()
server_socket.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
