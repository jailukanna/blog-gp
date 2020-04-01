---
title: socket example my application (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'my application'


Modules used in program: 
* `import tornado.websocket`
* `import tornado.web`
* `import tornado.ioloop`
* `import sys`
* `import os`

## python my application

Python socket example: my application

```python
import os
import sys

import tornado.ioloop
import tornado.web
import tornado.websocket

from tornado.options import define, options, parse_command_line

define("port", default=8888, help="run on the given port", type=int)

TEMPLATE_DIR = os.path.join(os.path.dirname(os.path.abspath(__file__)), 'templates')


class IndexHandler(tornado.web.RequestHandler):

    @tornado.web.asynchronous
    def get(self):
        self.render(os.path.join(TEMPLATE_DIR, "index.html"))


clients = dict()

rooms = dict()

class WebSocketHandler(tornado.websocket.WebSocketHandler):

    def open(self, room_id=None, *args, **kwargs):
        self.stream.set_nodelay(True)
        self.room_id = room_id
        self.join()

    def join(self):
        self.access_token = access_token = self.get_argument('access_token', None)
        clients[access_token] = {self.room_id: self}
        room = rooms.setdefault(self.room_id, set())
        room.add(access_token)
        self.application.on_room_changed()
        self.broadcast("{id} just joined the room".format(id=access_token))

    def broadcast(self, message):
        room = rooms[self.room_id]
        for client_id in room:
            connections = clients[client_id]
            connections[self.room_id].write_message(message)

    def on_message(self, message):
        self.broadcast(message)

    def on_close(self):
        if self in clients:
            self.clients.remove(self)

        if self.room_id in rooms:
            rooms[self.room_id].discard(self.access_token)

        self.application.on_room_changed()


class MyApplication(tornado.web.Application):
    def __init__(self, *args, **kwargs):
        super(MyApplication, self).__init__(*args, **kwargs)
        self.room_change_callbacks = set()

    def on_room_changed(self):
        loop = tornado.ioloop.IOLoop.current()
        for cb in self.room_change_callbacks:
            loop.add_callback(cb)

app = MyApplication([
    (r'/', IndexHandler),
    (r'/rt/(?P<room_id>[a-zA-Z0-9\.:,_]+)/?', WebSocketHandler),
])

if __name__ == '__main__':
    parse_command_line()
    app.listen(options.port)
    tornado.ioloop.IOLoop.instance().start()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
