---
title: matplotlib example DefaultImports (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'DefaultImports'


Modules used in program: 
* `import matplotlib.pylab as pylab`
* `import QuickPlot`
* `import sys, os`
* `import pdb`
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import matplotlib`

## python DefaultImports

Python matplotlib example: DefaultImports

```python
# For Python files.
# Created 2017, Zack Gainsforth
import matplotlib
#matplotlib.use('Qt4Agg')
import matplotlib.pyplot as plt
import numpy as np
from rich.traceback import install; install()
import pdb
#pdb.set_trace()

#And for IPython Notebooks:
# Created 2017, Zack Gainsforth
%pylab inline
import sys, os
import QuickPlot
import matplotlib.pylab as pylab
pylab.rcParams['figure.figsize'] = 8, 6  # that's default image size for this interactive session
from IPython.core.display import display, HTML
display(HTML("<style>.container { width:100% !important; }</style>"))
from ipywidgets.widgets import interactive, fixed, interact
%config InlineBackend.figure_format = 'retina'
from rich.traceback import install
install()

# At end of notebook you can automatically save it:
!jupyter-nbconvert --to html 'Plot Bruker Stack.ipynb'

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
