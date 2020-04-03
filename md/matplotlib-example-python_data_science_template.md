---
title: matplotlib example python data science template (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'python data science template'


Modules used in program: 
* `import altair as alt`
* `import seaborn as sns; `
* `import matplotlib.pyplot as plt`
* `import warnings`
* `import tqdm`
* `import numpy as np`
* `import pandas as pd`
* `import os`

## python python data science template

Python matplotlib example: python data science template

```python
#######################################################################
# Load standard libraries
#######################################################################
import os
import pandas as pd
import numpy as np
import tqdm
import warnings
warnings.filterwarnings('ignore')
from glob import glob
from IPython.core.interactiveshell import InteractiveShell
InteractiveShell.ast_node_interactivity = "all"
%config Completer.use_jedi = False
pd.options.display.max_rows = 999
pd.options.display.max_columns = 999 
pd.options.display.float_format = '{:20,.2f}'.format
from IPython.core.display import display, HTML
display(HTML("<style>.container { width:90% !important; }</style>"))

#######################################################################
# Load machine learning 
#######################################################################
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import Imputer
from sklearn.ensemble import RandomForestRegressor
from sklearn.ensemble import GradientBoostingRegressor
from sklearn.metrics import mean_squared_error, mean_absolute_error
from sklearn.pipeline import make_pipeline

#######################################################################
# Load data visualizations
#######################################################################
%config InlineBackend.figure_format = 'retina' # Set as High-Definition (HD) Image

# matplotlib
import matplotlib.pyplot as plt
%matplotlib inline

# seaborn
import seaborn as sns; 
sns.set(style="whitegrid", color_codes=True)

# bokeh
from bokeh.plotting import figure, output_file, show

# spotify chartify
# https://github.com/spotify/chartify

# altair
import altair as alt

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
