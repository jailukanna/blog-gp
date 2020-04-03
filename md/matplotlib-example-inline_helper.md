---
title: matplotlib example inline helper (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'inline helper'

Functions in program: 
* `def ioff():`
* `def inline():`

Modules used in program: 
* `import warnings`
* `import matplotlib.pyplot as plt`
* `import matplotlib`
* `import inline_helper`
* `import matplotlib.pyplot as plt`

## python inline helper

Python matplotlib example: inline helper

```python
"""Helper module that reassigns `matplotlib.pyplot.ioff` to use
in Jupyter/Ipython notebooks and assigns `matplotlib.pyplot.inline`
to use inline mode in notebooks. It allows notebooks to be exported
to scripts easily, as it doesn't use magic.

In notebooks:
import matplotlib.pyplot as plt
import inline_helper

#Turn inline on, this is interactive
#Equivalent to %matplotlib inline
plt.inline()

# Turn inline off and interactive off
# Equivalent to:
# %matplotlib
# plt.ioff()  #original function
plt.ioff()

#Turn interactive on
plt.ion()
"""

import matplotlib
import matplotlib.pyplot as plt
import warnings
ISHELL = None

try:
    import IPython
    ISHELL = IPython.get_ipython()
except:
    warnings.warn("IPython not Available")
    
def inline():
    """Turn inline mode on. Customized function."""
    if ISHELL:
        ISHELL.enable_matplotlib(gui='inline')
    else:
        warnings.warn("inline mode not available")
        
plt.inline = inline
    
def ioff():
    """Turn interactive mode off. Customized function that also turns
    off IPython inline mode if working in notebook."""
    if ISHELL:
        ISHELL.enable_matplotlib()
    matplotlib.interactive(False) #this is the call on matplotlib.pyplot
    
plt.ioff = ioff


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
