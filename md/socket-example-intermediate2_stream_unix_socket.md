---
title: socket example intermediate2 stream unix socket (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'intermediate2 stream unix socket'

Functions in program: 
* `def main():`

Modules used in program: 
* `import os`
* `import asyncio`

## python intermediate2 stream unix socket

Python socket example: intermediate2 stream unix socket

```python
#!/usr/bin/env python3
# encoding=utf-8
import asyncio
import os
from run_ import run
"""
filename: intermediate2_stream_unix_socket.py
description: asyncio unix socket
License: GPL V2
Author: 天使de眼睛

除特别声明，所有代码均是 python3.6 在 iOS 环境下编写测试。
"""

asyncio.run = run

PATH = 'test.sock'
BUFFER = 1024

async def server_callback(reader, writer):
    message = await reader.readline()
    print(f"server received message: [{message.decode().strip()}]")
    print(f"server response message to client: [Hello {message.decode().strip()}]")
    writer.writelines(['Hello '.encode(), message])
    await writer.drain()

    print("close server")
    writer.close()

async def client(message):
    reader, writer = await asyncio.open_unix_connection(PATH)

    print(f'client send message: [{message.strip()}]')
    writer.writelines([message.encode()])

    data = await reader.readline()
    print(f'client received response from server: [{data.decode().strip()}]')

    print('close client')
    writer.close()

async def server_client():
    server = await asyncio.start_unix_server(server_callback, PATH)
    print(f'socket server running, path: {PATH}\n')

    await client('Kitty\n')
    server.close()

def main():
    asyncio.run(server_client())
    if os.path.exists(PATH):
        os.unlink(PATH)

if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
