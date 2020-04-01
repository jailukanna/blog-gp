---
title: socket example socket (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket'


## python socket

Python socket example: socket

```python
(pypyenv)kracekumar@aadukalam:~/codes/brubeck/demos$ ../pypyenv/bin/pypy demo_minimal.py 
Traceback (most recent call last):
  File "app_main.py", line 51, in run_toplevel
  File "demo_minimal.py", line 3, in <module>
    from brubeck.request_handling import Brubeck, WebMessageHandler
  File "/home/kracekumar/codes/brubeck/pypyenv/site-packages/brubeck-0.4.0-py2.7.egg/brubeck/request_handling.py", line 41, in <module>
    raise EnvironmentError('You need to install eventlet or gevent')
EnvironmentError: You need to install eventlet or gevent
    'tcp://127.0.0.1:9998'),
  File "/home/kracekumar/codes/brubeck/pypyenv/site-packages/brubeck-0.4.0-py2.7.egg/brubeck/connections.py", line 142, in __init__
    zmq = load_zmq()
  File "/home/kracekumar/codes/brubeck/pypyenv/site-packages/brubeck-0.4.0-py2.7.egg/brubeck/connections.py", line 103, in load_zmq
    from eventlet.green import zmq
  File "/home/kracekumar/codes/brubeck/pypyenv/site-packages/eventlet/green/zmq.py", line 3, in <module>
    __zmq__ = __import__('zmq')
  File "/home/kracekumar/codes/brubeck/pypyenv/site-packages/zmq/__init__.py", line 1, in <module>
    configdir = py.path.local.make_numbered_dir(prefix='ctypes_configure')
  File "/home/kracekumar/codes/brubeck/pypyenv/site-packages/py/_path/local.py
", line 667, in make_numbered_dir
    rootdir = cls.get_temproot()
  File "/home/kracekumar/codes/brubeck/pypyenv/site-packages/py/_path/local.py
", line 646, in get_temproot
    return py.path.local(py.std.tempfile.gettempdir())
  File "/home/kracekumar/codes/pypytest/pypy-1.9/lib-python/2.7/tempfile.py", line 254, in gettempdir
    tempdir = _get_default_tempdir()
  File "/home/kracekumar/codes/pypytest/pypy-1.9/lib-python/2.7/tempfile.py", line 192, in _get_default_tempdir
    fp.close()
  File "/home/kracekumar/codes/brubeck/pypyenv/site-packages/eventlet/greenio.py", line 418, in close
    super(GreenPipe, self).close()
  File "/home/kracekumar/codes/pypytest/pypy-1.9/lib-python/2.7/socket.py", line 327, in close
    self._sock._decref_socketios()
AttributeError: '_SocketDuckForFd' object has no attribute '_decref_socketios'

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
