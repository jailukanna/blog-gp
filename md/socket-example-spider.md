---
title: socket example spider (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'spider'

Functions in program: 
* `def app(environ, start_response):`

## python spider

Python socket example: spider

```python
KB = 1024
MB = 1024*KB
GB = 1024*MB

fd = open('/dev/urandom')

def app(environ, start_response):
    start_response('200 OK', [])
    client_fd = environ['wsgi.input']
    while True:
        junk = fd.read(100 * MB)
        client_fd.write(junk)
        client_fd.flush()

if __name__ == '__main__':
    from wsgiref.simple_server import make_server
    server = make_server('127.0.0.1', 80, app)
    server.serve_forever()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
