---
title: socket example websocket client (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'websocket client'


Modules used in program: 
* `import _thread as thread`
* `import websocket`
* `import json`

## python websocket client

Python socket example: websocket client

```python
import json
from time import sleep, time
import websocket
import _thread as thread

ENGINEIO_OPEN = "0"
ENGINEIO_PING = "2"
ENGINEIO_PONG = "3"
ENGINEIO_MESSAGE = "4"
ENGINEIO_IGNORABLE = frozenset((ENGINEIO_OPEN, ))

SOCKETIO_OPEN = "0"
SOCKETIO_EVENT = "2"
SOCKETIO_IGNORABLE = frozenset((SOCKETIO_OPEN, ))

class SocketIOClient(object):
    def __init__(self, ws_url, ping_interval=None, ping_timeout=None, trace=False):
        self.ws_url = ws_url
        websocket.enableTrace(trace)
        self.ws = websocket.WebSocketApp(self.ws_url,
                                  on_message=self.on_message,
                                  on_open=self.on_open,
                                  on_error=self.on_error,
                                  on_close=self.on_close)

        self.ping_interval = ping_interval
        self.ping_timeout = ping_timeout
        self.last_pong = None
        self.keepalive_thread = None
        self.callbacks = {}

    def set_callbacks(self, callbacks):
        self.callbacks = callbacks

    def run_forever(self):
        self.last_pong = time()
        self.start_keepalive()
        self.ws.run_forever()

    def on_message(self, ws, message):
        if len(message) > 0:
            print("EngineIO message", message[:64])
            if message[0] == ENGINEIO_MESSAGE:
                self.handle_socketio_message(message[1:])
            elif message[0] == ENGINEIO_PONG:
                print("EngineIO PONG")
                self.last_pong = time()
            elif message[0] in ENGINEIO_IGNORABLE:
                pass
            else:
                print("Unhandled message type", message[0], message[:64], "...")
        else:
            print("Odd, got an empty message")

    def handle_socketio_message(self, message):
        print("socket.io message", message[:64])
        if len(message) > 0:
            if message[0] == SOCKETIO_EVENT:
                self.handle_socketio_event(message[1:])
            elif message[0] in ENGINEIO_IGNORABLE:
                pass
            else:
                print("Unhandled socket.io message type", message[0], message[:64], "...")
        else:
            print("Empty socket.io message")

    def handle_socketio_event(self, message):
        try:
            parsed_event = json.loads(message)
            event_name, payload = parsed_event
            if event_name in self.callbacks:
                self.callbacks[event_name](self.ws, event_name, payload)
        except json.JSONDecodeError as err:
            if "error" in self.callbacks:
                self.callbacks["error"](self.ws, err, message)

    def on_open(self, ws):
        print("Socket opened")
        if "open" in self.callbacks:
            self.callbacks["open"](self.ws)
        if self.ping_thread is None and self.ping_interval is not None:
            self.start_keepalive()

    def on_error(self, ws, error):
        print("Error:", error)
        if "error" in self.callbacks:
            self.callbacks["error"](self.ws)

    def on_close(self, ws):
        print("### closed ###")
        if "close" in self.callbacks:
            self.callbacks["close"](self.ws)
        if self.auto_reconnect:
            self.run_forever()

    def start_keepalive(self):
        def run():
            while True:
                sleep(self.ping_interval)
                print("Shutoff timeout is", self.ping_interval + self.ping_timeout, "time since last pong is", time() - self.last_pong)
                if time() - self.last_pong < self.ping_interval + self.ping_timeout:
                    print("Sending a ping")
                    self.ws.send(ENGINEIO_PING)
                else:
                    print("Ping timeout, disconnect")
                    self.ws.close()
                    self.last_pong = time()
            print("Ping thread terminating")
        self.ping_thread = thread.start_new_thread(run, ())

ED_WS_URL = "wss://socket.etherdelta.com/socket.io/?transport=websocket"

# These should be set from server settings, but are unlikely to change
PING_INTERVAL = 25 # seconds
PING_TIMEOUT = 60

if __name__ == "__main__":
    def on_orders(ws, event_name, payload):
        print("Orders are here!", payload)

    def on_trades(ws, event_name, payload):
        print("Just some trades:", payload)

    instance = SocketIOClient(ED_WS_URL, PING_INTERVAL, PING_TIMEOUT, trace=True)
    instance.set_callbacks({ "orders": on_orders, "trades": on_trades })
    instance.run_forever()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
