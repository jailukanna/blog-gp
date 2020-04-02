---
title: pil example showarray (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'showarray'

Functions in program: 
* `def showarray(a, fmt='png'):`

Modules used in program: 
* `import numpy as np`
* `import IPython.display`
* `import PIL.Image`

## python showarray

Python pil example: showarray

```python
import PIL.Image
from cStringIO import StringIO
import IPython.display
import numpy as np
def showarray(a, fmt='png'):
    a = np.uint8(a)
    f = StringIO()
    PIL.Image.fromarray(a).save(f, fmt)
    IPython.display.display(IPython.display.Image(data=f.getvalue()))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
