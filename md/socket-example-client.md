---
title: socket example client (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'client'

Functions in program: 
* `def main():`

Modules used in program: 
* `import sys`
* `import socket`

## python client

Python socket example: client

```python
import socket
import sys

def main():
    soc = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    host = "127.0.0.1"
    port = 8888

    try:
        soc.connect((host, port))
    except:
        print("Connection error")
        sys.exit()

    print("Enter 'quit' to exit")
    message = input(" -> ")

    while message != 'quit':
        soc.sendall(message.encode("utf8"))
        if soc.recv(5120).decode("utf8") == "-":
            pass        # null operation

        message = input(" -> ")

    soc.send(b'--quit--')

if __name__ == "__main__":
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
