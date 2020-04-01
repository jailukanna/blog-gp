---
title: socket example demo2 (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'demo2'


## python demo2

Python socket example: demo2

```python
async def handle_client(reader, writer):
    request = None
    while request != 'quit':
        request = (await reader.read(255)).decode('utf8')
        response = str(eval(request)) + '\n'
        writer.write(response.encode('utf8'))

loop = asyncio.get_event_loop()
loop.create_task(asyncio.start_server(handle_client, 'localhost', 15555))
loop.run_forever()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
