---
title: socket example fuzzer2 (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'fuzzer2'

Functions in program: 
* `def f(i):`

Modules used in program: 
* `import random,gdb`

## python fuzzer2

Python socket example: fuzzer2

```python
import random,gdb

g=gdb.gdb()

def f(i):
        if i==0:
                for c in range(32,127):
                        yield chr(c)
        else:
                yield from f(i-1)
                for c in range(32,127):
                        for _ in f(i-1):
                                yield chr(c)+_


for _ in f(8):
    if not any([_.startswith(__) for __ in ['p','P','s','z','Z','q','m','M','e']]):
        continue
    print(_)
    g.send(_)
    g.recv_msg()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
