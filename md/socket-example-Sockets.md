---
title: socket example Sockets (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'Sockets'


## python Sockets

Python socket example: Sockets

```python
class Server:
    def __init__(self, address, port):
        self.socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.bound_address = (address, port)
        self.socket.bind(self.bound_address)

        print(f"Creating server at {address}:{port}")

    def __del__(self):
        self.connection.close()
        print("Closed socket connection")

    def start(self):
        self.socket.listen(1)
        self.connection = self.socket.accept()[0]

        while True:
            try:
                data = self.connection.recv(1024)

                if data:
                    print(data.decode())

            except:
                traceback.print_exc()
                break


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
