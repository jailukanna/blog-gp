---
title: matplotlib example jupyter imports (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'jupyter imports'


Modules used in program: 
* `import os`
* `import sys`
* `import matplotlib`
* `import matplotlib.pyplot as plt`
* `import pandas as pd`
* `import numpy as np`

## python jupyter imports

Python matplotlib example: jupyter imports

```python
# Autoreload modules when they change
%load_ext autoreload
%autoreload 2

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib

import sys
import os
from pathlib import Path

# Progress bars for jupyter
from tqdm import tqdm_notebook as tqdm

# Debugging in jupyter
from IPython.core.debugger import set_trace

# Show all columns in pandas dataframes
from IPython.display import display
pd.options.display.max_columns = None

# Importing modules in pycharm, not good, better read this https://stackoverflow.com/questions/19885821/how-do-i-import-modules-in-pycharm
module_path = os.path.abspath(os.path.join('./sim_results_tools'))
if module_path not in sys.path:
    sys.path.append(module_path)
from sim_results_tools.misc_funtions import haversine_np, getFromDict,  remove_duplicates_ordered
from sim_results_tools.log_import import count_log_files

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
