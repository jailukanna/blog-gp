---
title: socket example get (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'get'


Modules used in program: 
* `import socket`

## python get

Python socket example: get

```python
import socket

class Get:
    """
        this class makes get request to httpbin
    """

    def __init__(self):
        self.s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR,0)

        self.request = ("GET /get HTTP/1.1\n"
                        "Host: httpbin.org\n"
                        "User-Agent:Mozilla 5.0\n\n"
                        )

    def make_request(self):
        self.s.connect(('httpbin.org',80))
        self.s.send(self.request.encode('utf-8'))
        self.response = self.s.recv(4096)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
