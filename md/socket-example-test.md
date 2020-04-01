---
title: socket example test (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'test'


Modules used in program: 
* `import unittest`

## python test

Python socket example: test

```python
import unittest
from my_application import app, rooms
from tornado import testing, websocket, gen


class WebSocketServerTests(testing.AsyncHTTPTestCase):
    def setUp(self):
        super(WebSocketServerTests, self).setUp()
        self.base_url = '/rt/{room_id}/?access_token={token}'

    def get_app(self):
        return app

    def get_protocol(self):
        return 'ws'

    @testing.gen_test
    def test_room_registry(self):

        room_id = '1'
        token = '1'
        url = self.base_url.format(room_id=room_id, token=token)

        conn1 = yield websocket.websocket_connect(self.get_url(url),
                                                  io_loop=self.io_loop)

        message = yield conn1.read_message()
        self.assertIn(token, rooms[room_id])

        callback = yield gen.Callback('change')
        self._app.room_change_callbacks.add(callback)
        conn1.protocol.close()
        yield gen.Wait('change')
        self._app.room_change_callbacks.remove(callback)
        self.assertNotIn(token, rooms[room_id])


if __name__ == '__main__':
    unittest.main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
