---
title: socket example echoserver1 (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'echoserver1'


## python echoserver1

Python socket example: echoserver1

```python
#!/usr/bin/env python3
# Simple little echo server for python 3.

from socketserver import ThreadingMixIn, TCPServer, StreamRequestHandler

class Server(ThreadingMixIn, TCPServer):
    allow_reuse_address = True

class ConnectionHandler(StreamRequestHandler):
    def handle(self):
        while True:
            line = self.rfile.readline()
            if not line: break
            print("Received from connection:", line)
            self.wfile.write(line)

if __name__ == '__main__':
    Server(('0.0.0.0', 5999), ConnectionHandler).serve_forever()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
