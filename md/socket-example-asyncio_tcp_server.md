---
title: socket example asyncio tcp server (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'asyncio tcp server'


Modules used in program: 
* `import asyncio`

## python asyncio tcp server

Python socket example: asyncio tcp server

```python
import asyncio

class Server(asyncio.Protocol):

    def __init__(self, app=None) -> None:
        self.app = app

    def connection_made(self, transport: asyncio.BaseTransport) -> None:
        self.transport = transport

    def data_received(self, data: bytes) -> None:
        message = data.decode()
        print(message)
        if self.app.t:
            print('True')
            self.app.test()
        else:
            print('False')
            self.app.test()


class Application:

    def __init__(self) -> None:
        self.t = False

    def test(self) -> None:
        self.t = not self.t

    async def _run(self) -> None:
        loop = asyncio.get_running_loop()
        self.server = await loop.create_server(lambda: Server(app=self), 'localhost', 9797)
        await self.server.start_serving()
        # await self.server.serve_forever()

    def run(self) -> None:
        asyncio.run(self._run())


app = Application()
app.run()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
