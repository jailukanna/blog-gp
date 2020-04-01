---
title: socket example crossdomain (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'crossdomain'


Modules used in program: 
* `import SocketServer`

## python crossdomain

Python socket example: crossdomain

```python
import SocketServer

class ThreadedTCPRequestHandler(SocketServer.BaseRequestHandler):

    def handle(self):
        self.request.sendall("""

<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE cross-domain-policy SYSTEM "http://www.adobe.com/xml/dtds/cross-domain-policy.dtd">
<cross-domain-policy><site-control permitted-cross-domain-policies="all"/>
<allow-access-from domain="*.aifang.com" to-ports="*" />
<allow-access-from domain="*.pages.aifcdn.com" to-ports="*" />
<allow-access-from domain="*.aifcdn.com" to-ports="*" />
<allow-access-from domain="anjuke.adsame.com" to-ports="*" />
<allow-access-from domain="anjuke.adsame.com" to-ports="*" />
</cross-domain-policy>
""")

class ThreadedTCPServer(SocketServer.ThreadingMixIn, SocketServer.TCPServer):
    allow_reuse_address = True

if __name__ == "__main__":

    HOST, PORT = "", 843
    server = ThreadedTCPServer((HOST, PORT), ThreadedTCPRequestHandler)
    try:
        server.serve_forever()
    except KeyboardInterrupt:
        pass


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
