---
title: socket example tornado ws (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'tornado ws'

Functions in program: 
* `def redis_listener():`

Modules used in program: 
* `import threading`
* `import redis`
* `import os`
* `import tornado.web`
* `import tornado.ioloop`
* `import tornado.websocket`
* `import tornado.httpserver`

## python tornado ws

Python socket example: tornado ws

```python
#! encoding:utf8
import tornado.httpserver
import tornado.websocket
import tornado.ioloop
import tornado.web
import os
import redis
import threading

LISTENERS = []

REDIS_HOST = os.environ.get('REDIS_HOST', '192.168.5.133')
REDIS_PORT = os.environ.get('REDIS_PORT', 6379)
REDIS_DB = 0
TASK_CHANNEL = 'task_flow'
TORNADO_RUN_IP = os.environ.get('TORNADO_RUN_IP', '192.168.5.133')
TORNADO_RUN_PORT = os.environ.get('TORNADO_RUN_PORT', 1234)


def redis_listener():
    conn = redis.Redis(host=REDIS_HOST, port=REDIS_PORT, db=REDIS_DB)
    ps = conn.pubsub()
    ps.subscribe('task_flow')
    # redis 订阅者
    for message in ps.listen():
        for element in LISTENERS:
            element.write_message(unicode(message['data']))


class SocketHandler(tornado.websocket.WebSocketHandler):

    def check_origin(self, origin):
        return True

    def open(self):
        print(">>> new connection >>>")
        LISTENERS.append(self)

    def on_message(self, message):
        pass

    def on_close(self):
        LISTENERS.remove(self)
        print("connection closed!")


application = tornado.web.Application([
    (r'/ws', SocketHandler),
])

if __name__ == "__main__":
    threading.Thread(target=redis_listener).start()
    http_server = tornado.httpserver.HTTPServer(application)
    http_server.listen(TORNADO_RUN_PORT, TORNADO_RUN_IP)
    print('Tornado web socket server is running ...')
    tornado.ioloop.IOLoop.instance().start()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
