---
title: socket example demo selectors (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'demo selectors'


Modules used in program: 
* `import selectors`
* `import socket`
* `import sys`

## python demo selectors

Python socket example: demo selectors

```python
import sys
import socket
import selectors

HOST = '127.0.0.1'
PORT = 9999

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as sock:
    sock.bind((HOST, PORT))
    sock.listen(5)
    print("Server is listening at {}:{}".format(HOST, PORT))
    sock.setblocking(False)

    with selectors.DefaultSelector() as selector:
        selector.register(sock, selectors.EVENT_READ)

        try:
            while True:
                ready = selector.select()

                if not ready:
                    break

                for key, _ in ready:
                    if key.fileobj == sock:
                        try:
                            connection, client_address = sock.accept()
                            print("Accepted from", client_address)
                        except:
                            sys.exc_info()[1]
                        else:
                            selector.register(connection, selectors.EVENT_READ)
                    else:
                        connection = key.fileobj

                        raw_data = connection.recv(1024)

                        if not raw_data:
                            print("Closed connection to", connection.getpeername())
                            selector.unregister(connection)
                            connection.close()
                            break

                        print("Received {} from {}".format(raw_data.decode(), connection.getpeername()))
        except KeyboardInterrupt:
            print("EXIT")
        except:
            sys.exc_info()[1]

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
