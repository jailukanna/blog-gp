---
title: socket example demo select (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'demo select'


Modules used in program: 
* `import select`
* `import socket`
* `import sys`

## python demo select

Python socket example: demo select

```python
import sys
import socket
import select

HOST = '127.0.0.1'
PORT = 9999

fd_to_socket = {}

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as sock:
    sock.bind((HOST, PORT))
    sock.listen(5)
    print("Server is listening at {}:{}".format(HOST, PORT))

    sock.setblocking(False)

    poll = select.poll()
    poll.register(sock, select.POLLIN)

    try:
        while True:
            ready = poll.poll()

            if not ready:
                break

            for fd, event in ready:
                if fd == sock.fileno():
                    connection, client_address = sock.accept()
                    print("Accepted from", client_address)

                    fd_to_socket[connection.fileno()] = connection
                    poll.register(connection, select.POLLIN)
                else:
                    connection = fd_to_socket[fd]
                    raw_data = connection.recv(1024)

                    if not raw_data:
                        print("Closed connection to", connection.getpeername())
                        poll.unregister(connection)
                        connection.close()
                        break

                    print("Reveived {} from {}".format(raw_data.decode(), connection.getpeername()))
    except KeyboardInterrupt:
        print("EXIT")
    except:
        print(sys.exc_info()[1])

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
