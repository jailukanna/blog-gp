---
title: socket example socket connector (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket connector'


## python socket connector

Python socket example: socket connector

```python
'''
This is a minimal python chat client which connects to the `rooms:lobby` topic.

The server is supposed to be
http://www.phoenixframework.org/docs/channels#section-tying-it-all-together
'''
from __future__ import print_function

try:  # py2
    get_user_input = raw_input
except:  # py3
    get_user_input = input

from occamy import Socket

PING_COMMAND = 'ping'

socket = Socket('ws://kre.vagrant.internal:8102/socket',
                params={'session_id': 'nBt0taU5vzxR68bac8addc9634e4714ed162b1140cf537fd5ae6CVzEX8r9BJt7uXeOwpXUg',
                        'instance': 'brewfictus'})
socket.connect()

channel = socket.channel('presence-281f395f6f51d031a6d3db3489906c98285191ebac41bb744f9323f61af63433@v1_users_1', {})
channel.on('new', lambda msg, x: print('> {}'.format(msg)))
channel.on('pong', lambda msg, x: print('> {}'.format(msg)))
channel.on('presence_state', lambda msg, x: print('> {}'.format(msg)))
channel.on('presence_diff', lambda msg, x: print('> {}'.format(msg)))
channel.on('CHANGE', lambda msg, x: print('> {}'.format(msg)))

channel.join()

print('Enter your message and press return to send the message.')
print()

while True:
    message = raw_input('')
    args = message.split(' ')
    command = args[0]
    if command == PING_COMMAND:
        body = args[1]
        channel.push(command, {'body': body})


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
