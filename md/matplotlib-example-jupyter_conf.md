---
title: matplotlib example jupyter conf (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'jupyter conf'


Modules used in program: 
* `import warnings`
* `import seaborn as sns`
* `import matplotlib as mpl`
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import pandas as pd`

## python jupyter conf

Python matplotlib example: jupyter conf

```python
# for Python 2 compability
from __future__ import division, print_function
%matplotlib inline
import pandas as pd
import numpy as np

import matplotlib.pyplot as plt
import matplotlib as mpl
import seaborn as sns

# disable Anaconda warnings
import warnings
warnings.filterwarnings('ignore')

# plots params
mpl.rcParams['figure.figsize'] = (15, 8)
mpl.rcParams['font.size'] = 14
mpl.rcParams['axes.labelsize'] = 14
mpl.rcParams['xtick.labelsize'] = 14
mpl.rcParams['ytick.labelsize'] = 14

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
