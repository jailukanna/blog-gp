---
title: socket example py-req-die (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'py-req-die'


Modules used in program: 
* `import zmq`

## python py-req-die

Python socket example: py-req-die

```python
#!/bin/env python

import zmq

ctx = zmq.Context(1)

s = ctx.Socket(zmq.REQ)
s.connect("tcp://127.0.0.1:9005")

s.send("WOOT")

exit(127) # Die a horrible death ...


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
