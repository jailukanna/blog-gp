---
title: pil example ev3imports (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'ev3imports'


Modules used in program: 
* `import socket`

## python ev3imports

Python pil example: ev3imports

```python
import socket
hostname = socket.gethostname()

if hostname == 'ev3dev':
    # We are on the EV3, import required modules directly
    import ev3dev.ev3   as ev3
    import ev3dev.fonts as fonts
    import PIL
else:
    # We are somewhere else. Assume we are using RPyC:
    import rpyc
    conn = rpyc.classic.connect('ev3dev')

    ev3   = conn.modules['ev3dev.ev3']
    fonts = conn.modules['ev3dev.fonts']
    PIL   = conn.modules['PIL']


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
