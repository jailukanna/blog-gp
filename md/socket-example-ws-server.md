---
title: socket example ws-server (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'ws-server'

Functions in program: 
* `def main():`
* `def sendMessage(client, message):`
* `def isDeviceOnline():`

Modules used in program: 
* `import os.path`
* `import logging`
* `import tornado.ioloop`
* `import tornado.web`
* `import tornado.websocket`

## python ws-server

Python socket example: ws-server

```python
import tornado.websocket
import tornado.web
import tornado.ioloop
import logging
import os.path
from tornado.options import define, options

define("port", default=8080, help="run on the given port", type=int)
define("debug", default=True, help="run in debug mode")


class Application(tornado.web.Application):
    def __init__(self):
        handlers = [
            (r"/", MainHandler),
            (r"/chatsocket", ChatSocketHandler),
        ]
        settings = dict(
            cookie_secret="__TODO:_GENERATE_YOUR_OWN_RANDOM_VALUE_HERE__",
            template_path=os.path.join(os.path.dirname(__file__), "views"),
            static_path=os.path.join(os.path.dirname(__file__), "static"),
            xsrf_cookies=False,
            debug=options.debug,
        )
        super(Application, self).__init__(handlers, **settings)


class MainHandler(tornado.web.RequestHandler):
    def get(self):
        self.render("index.html")


class ChatSocketHandler(tornado.websocket.WebSocketHandler):
    controller = None
    device = None

    def open(self):
        logging.info('Client connected')

    def on_message(self, message):
        logging.info('Message received: %s', message)

        if 'device' == message:
            ChatSocketHandler.device = self
        elif 'controller' == message:
            ChatSocketHandler.controller = self

        if self == ChatSocketHandler.controller:
            sendMessage(ChatSocketHandler.controller, isDeviceOnline())
            if ChatSocketHandler.device is not None:
                sendMessage(ChatSocketHandler.device, message)
        elif self == ChatSocketHandler.device:
            if ChatSocketHandler.controller is None:
                logging.info('Controller is missing')
            else:
                sendMessage(ChatSocketHandler.controller, isDeviceOnline())
        else:
            logging.warn('Urgent! Somebody broke our super security connection!!!')

    def data_received(self, chunk):
        logging.info("data_received")

    def on_close(self):
        if ChatSocketHandler.controller == self:
            logging.info('Controller leave')
            ChatSocketHandler.controller = None
        elif ChatSocketHandler.device == self:
            logging.info('Device leave')
            ChatSocketHandler.device = None
            if ChatSocketHandler.controller is not None:
                sendMessage(ChatSocketHandler.controller, isDeviceOnline())


def isDeviceOnline():
    return 'online' if ChatSocketHandler.device is not None else 'offline'


def sendMessage(client, message):
    try:
        client.write_message(message)
    except:
        logging.error("Error sending message", exc_info=True)


def main():
    tornado.options.parse_command_line()
    app = Application()
    app.listen(options.port)
    tornado.ioloop.IOLoop.current().start()


if __name__ == "__main__":
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
