---
title: socket example utils (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'utils'

Functions in program: 
* `def readlines(sock, buffer_size=2048, delim='\n'):`

## python utils

Python socket example: utils

```python
# -*- coding: utf-8 -*-
"""
Some useful function.
"""


def readlines(sock, buffer_size=2048, delim='\n'):
    """
    Read data from socket until connection is closed,
    and supply a generator interface.
    """
    buf = ''
    data = True
    while data:
        data = sock.recv(buffer_size)
        buf += data.decode()

        while buf.find(delim) != -1:
            line, buf = buf.split('\n', 1)
            yield line
    return


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
