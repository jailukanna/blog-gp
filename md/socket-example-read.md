---
title: socket example read (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'read'


Modules used in program: 
* `import sys`

## python read

Python socket example: read

```python
#!/usr/bin/env python3

import sys

line = None
with open(sys.argv[1], "r") as f:
    line = f.readline()

print("from py: %s" % line)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
