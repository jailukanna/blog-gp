---
title: socket example common (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'common'

Functions in program: 
* `def check(status_code, headers, content):`

## python common

Python socket example: common

```python
URL = 'http://httpbin.org/status/200'

def check(status_code, headers, content):
    assert status_code == 200
    assert headers['Content-Length'] == '0'
    assert headers['Content-Type'] == 'text/html; charset=utf-8'
    assert headers['Connection'] == 'close'
    print('OK')


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
