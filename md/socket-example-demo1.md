---
title: socket example demo1 (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'demo1'


Modules used in program: 
* `import asyncio, socket`

## python demo1

Python socket example: demo1

```python
import asyncio, socket

async def handle_client(client):
    request = None
    while request != 'quit':
        request = (await loop.sock_recv(client, 255)).decode('utf8')
        response = str(eval(request)) + '\n'
        await loop.sock_sendall(client, response.encode('utf8'))
    client.close()

async def run_server():
    while True:
        client, _ = await loop.sock_accept(server)
        loop.create_task(handle_client(client))

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind(('localhost', 15555))
server.listen(8)
server.setblocking(False)

loop = asyncio.get_event_loop()
loop.run_until_complete(run_server())

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
