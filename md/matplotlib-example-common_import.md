---
title: matplotlib example common import (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'common import'


Modules used in program: 
* `import seaborn as sns`
* `import matplotlib`
* `import numpy as np`
* `import pandas as pd`
* `import sys`
* `import os`

## python common import

Python matplotlib example: common import

```python
import os
import sys

import pandas as pd
import numpy as np
import matplotlib
from matplotlib import pyplot as plt
import seaborn as sns

from IPython.core.display import display, HTML
display(
  HTML("<style>.container {width: 100% ! important;}</style>")
)

pd.set_option('display.max_rows', 50)
pd.set_option('display.max_columns', 100)
pd.set_option('display.width', 200)

%matplotlib inline
sns.set()

%load_ext autoreload
%autoreload 2

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
