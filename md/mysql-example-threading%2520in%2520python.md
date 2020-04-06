---
title: mysql example threading%2520in%2520python (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'threading%2520in%2520python'


Modules used in program: 
* `import threading`

## python threading%2520in%2520python

Python mysql example: threading%2520in%2520python

```python
import threading

class abhimessenger(threading.Thread):
    def run(self):
        for _ in range(10):
            print(threading.currentThread().getName())
x = abhimessenger(name = 'send out messages')
y = abhimessenger(name = 'recieve messages')

x.start()
y.start()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
