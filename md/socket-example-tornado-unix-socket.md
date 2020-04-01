---
title: socket example tornado-unix-socket (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'tornado-unix-socket'


Modules used in program: 
* `import tornado.web`
* `import tornado.ioloop`

## python tornado-unix-socket

Python socket example: tornado-unix-socket

```python
import tornado.ioloop
import tornado.web
from tornado.options import options, define
from tornado.netutil import bind_unix_socket

class MainHandler(tornado.web.RequestHandler):
    def get(self):
        self.write("Hello, world")
 
application = tornado.web.Application([
    (r"/", MainHandler),
])
 
define('listen_address', group='webserver', default='127.0.0.1', help='Listen address')
define('listen_port', group='webserver', default=8888, help='Listen port')
define('unix_socket', group='webserver', default=None, help='Path to unix socket to bind')
 
if __name__ == "__main__":
    if options.unix_socket:
        server = HTTPServer(application)
        socket = bind_unix_socket(options.unix_socket)
        server.add_socket(socket)
    else:
        application.listen(options.listen_port, address=options.listen_address)
    tornado.ioloop.IOLoop.instance().start()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
