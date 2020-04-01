---
title: socket example async usix socket client (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'async usix socket client'

Functions in program: 
* `def main():`

Modules used in program: 
* `import time`
* `import sys`
* `import itertools`
* `import json`
* `import asyncio`

## python async usix socket client

Python socket example: async usix socket client

```python
import asyncio
import json
import itertools
import sys
import time


MSG = dict(
        job_id='1111111111111',
        job_type='my_type',
        manager_type='local',
        arguments='log_txt_file=/path/to/test_log.txt'
    )
server_address = './uds_socket.s'


async def spin(msg):
    write, flush = sys.stdout.write, sys.stdout.flush
    for char in itertools.cycle('|/-\\'):
        status = char + ' ' + msg
        write(status)
        flush()
        write('\x08' * len(status))
        try:
            await asyncio.sleep(.1)
        except asyncio.CancelledError:
            break
    write(' ' * len(status) + '\x08' * len(status))


async def handle_client(msg):
    await asyncio.sleep(3)
    reader, writer = await asyncio.open_unix_connection(server_address)
    writer.write(str(json.dumps(msg)+'\r\n\r\n').encode('utf8'))
    answer = 'b'
    print(answer)
    while not answer.endswith('\r\n\r\n'):
        print('started')
        data = await reader.read(32)
        answer += data.decode('utf8')
    else:
        print(answer)
    writer.close()


async def supervisor():
    spinner = asyncio.ensure_future(spin('thinking!'))
    print('spinner object:', spinner)
    result = await handle_client(MSG)
    spinner.cancel()
    return result


def main():
    loop = asyncio.get_event_loop()
    result = loop.run_until_complete(supervisor())
    loop.close()
    print('Answer:', result)


if __name__ == '__main__':
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
