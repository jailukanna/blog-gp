---
title: matplotlib example import matplot (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'import matplot'


Modules used in program: 
* `import matplotlib.pyplot as plt`

## python import matplot

Python matplotlib example: import matplot

```python
from sys import platform as sys_pf

if sys_pf == 'darwin':
    # solve crashing issue https://github.com/MTG/sms-tools/issues/36
    import matplotlib

    matplotlib.use("TkAgg")
import matplotlib.pyplot as plt

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
