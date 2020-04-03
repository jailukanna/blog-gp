---
title: matplotlib example output to clipboard (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'output to clipboard'

Functions in program: 
* `def clip(line):`

## python output to clipboard

Python matplotlib example: output to clipboard

```python
## Send content to clipboard in ipython
from IPython.core.magic import register_line_magic
@register_line_magic
def clip(line):
    global_dict = globals()
    if not line in global_dict:
        return
    value = global_dict[line]
    import os    
    os.system("echo '%s' | pbcopy" % str(value))
    # os.system("echo '%s' | clip" % str(value)) # for windows
del clip

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
