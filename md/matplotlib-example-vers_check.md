---
title: matplotlib example vers check (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'vers check'


Modules used in program: 
* `import sys`

## python vers check

Python matplotlib example: vers check

```python

import sys

print('Versions\n')

print('python', sys.version)

try:
    import numpy
    print('numpy', numpy.__version__)
except:
    print('numpy not installed')

try:
    import matplotlib
    print('matplotlib', matplotlib.__version__)
except:
    print('matplotlib not installed')

try:
    import scipy
    print('scipy', scipy.__version__)
except:
    print('scipy not installed')

try:
    import rpy2
    print('rpy2', rpy2.__version__)
except:
    print('rpy2 not installed')


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
