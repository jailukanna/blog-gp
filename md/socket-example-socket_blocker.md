---
title: socket example socket blocker (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket blocker'

Functions in program: 
* `def my_req1():`
* `def my_req():`
* `def block_socket(f):`
* `def _blank(*args, **kwargs):`

Modules used in program: 
* `import requests`
* `import socket`

## python socket blocker

Python socket example: socket blocker

```python
# Based on https://github.com/miketheman/pytest-socket/blob/master/pytest_socket.py

import socket
import requests

_true_socket = socket.socket


def _blank(*args, **kwargs):
    raise ValueError('socket')


def block_socket(f):
    def new_f(*args, **kwargs):
        try:
            socket.socket = _blank
            f(*args, **kwargs)
        finally:
            socket.socket = _true_socket

    return new_f


def my_req():
    print(requests.get('https://docs.python.org/3/library/socket.html'))


@block_socket
def my_req1():
    print(requests.get('https://docs.python.org/3/library/socket.html'))


if __name__ == '__main__':
    my_req()
    # <Response [200]>
    try:
        my_req1()
    # Raises ValueError
    except ValueError as e:
        print("ValueError", e)
    my_req()
    # <Response [200]>


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
