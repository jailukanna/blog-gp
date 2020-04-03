---
title: matplotlib example libraries (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'libraries'


Modules used in program: 
* `import ipywidgets as widgets;`
* `import collections`
* `import itertools`
* `import scipy.io as sio`
* `import os`
* `import random`
* `import networkx as nx`
* `import warnings;`
* `import pylab as lp;`
* `import ipywidgets as widgets;`
* `import numpy as np`
* `import ipywidgets as widgets`
* `import pandas as pd`
* `import matplotlib.pyplot as plt`

## python libraries

Python matplotlib example: libraries

```python
%matplotlib inline
%config InlineBackend.close_figures=False;
import matplotlib.pyplot as plt
import pandas as pd
import ipywidgets as widgets
import numpy as np
from matplotlib import rcParams;
import ipywidgets as widgets;
from ipywidgets import Layout;
from IPython.display import display,clear_output;
import pylab as lp;
from pylab import *;
from scipy.ndimage import measurements;
from IPython.display import display,clear_output;
import warnings;
warnings.filterwarnings('ignore');
import networkx as nx
import random
import os
import scipy.io as sio
from scipy.io import loadmat
from pathlib import Path
import itertools
import collections
import ipywidgets as widgets;
from ipywidgets import Layout
from IPython.display import display,clear_output
from IPython.core.display import display, HTML
from pathlib import Path
from matplotlib import colors
from IPython.display import HTML as html_print
from IPython.display import display

out1 = widgets.Output()
out2 = widgets.Output()

button=widgets.Button(description='Next Spread')
buttonres=widgets.Button(description='Reset')
hbox=widgets.HBox(children=(button,buttonres))
vbox1=widgets.HBox(children=(out1,out2))
vbox=widgets.VBox(children=(vbox1,hbox))
display(vbox)
matplotlib.rcParams['figure.figsize'] = [6, 6]

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
