---
title: socket example py-rep (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'py-rep'


Modules used in program: 
* `import time`
* `import zmq`

## python py-rep

Python socket example: py-rep

```python
#!/bin/env python

import zmq
import time

ctx = zmq.Context(1)

s = ctx.Socket(zmq.REP)
s.bind("tcp://127.0.0.1:9005")

# Wait for a client to send something, and then die

time.sleep(10)
print(s.recv() # This is now "WOOT" eventhough the remote client is gone)

# Do expensive CPU cycle work here

# This gets sent into the ether, so why bother?
s.send("WOOT WOOT")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
