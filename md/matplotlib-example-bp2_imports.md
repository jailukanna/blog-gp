---
title: matplotlib example bp2 imports (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'bp2 imports'


Modules used in program: 
* `import pydotplus`
* `import lime`
* `import eli5`
* `import shap`
* `import pingouin as pg`
* `import xgboost as xgb`
* `import matplotlib.pyplot as plt`
* `import seaborn as sns`
* `import pickle`
* `import warnings`
* `import os, ast`
* `import numpy as np`
* `import pandas as pd`

## python bp2 imports

Python matplotlib example: bp2 imports

```python
# General
import pandas as pd
import numpy as np
import os, ast
pd.set_option('display.max_colwidth', -1)
from tqdm import tqdm_notebook
import warnings
warnings.filterwarnings('ignore')
import pickle


# Visualisation
import seaborn as sns
from IPython.display import display_html
import matplotlib.pyplot as plt
from statsmodels.graphics.gofplots import qqplot
from IPython.core.display import display, HTML


#Algorithms
import xgboost as xgb
from sklearn.tree import DecisionTreeRegressor
from sklearn.ensemble import RandomForestRegressor, GradientBoostingRegressor
from catboost import CatBoostRegressor
from lightgbm import LGBMRegressor
from sklearn.neighbors import KNeighborsRegressor
from sklearn.linear_model import Lasso
from sklearn.model_selection import RandomizedSearchCV, KFold, GridSearchCV
import pingouin as pg
from sklearn.metrics import r2_score

# Model Explainability
import shap
import eli5
import lime
from pdpbox import pdp, get_dataset, info_plots
from IPython.display import Image
from sklearn import tree
import pydotplus

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
