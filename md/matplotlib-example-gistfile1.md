---
title: matplotlib example gistfile1 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'gistfile1'


Modules used in program: 
* `import numpy as np`
* `import pandas as pd`
* `import seaborn as sns`
* `import matplotlib.pyplot as plt`

## python gistfile1

Python matplotlib example: gistfile1

```python
from __future__ import division

# graphics
import matplotlib.pyplot as plt
from matplotlib import rc_params

mpl_default = rc_params()

import seaborn as sns
sns.set(rc=mpl_default)

# needs to be called after seaborn.set() ?!?!
%matplotlib inline

# needs to be called after %matplotlib inline to override ipython styles
plt.rcParams = mpl_default

# numbers
import pandas as pd
import numpy as np

pd.set_option("display.max_columns", 101)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
