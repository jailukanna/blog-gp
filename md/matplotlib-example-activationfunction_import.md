---
title: matplotlib example activationfunction import (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'activationfunction import'


Modules used in program: 
* `import warnings`
* `import time`
* `import imageio`
* `import seaborn as sns`
* `import matplotlib.colors`
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python activationfunction import

Python matplotlib example: activationfunction import

```python
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.colors
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score, mean_squared_error, log_loss
from tqdm import tqdm_notebook
import seaborn as sns
import imageio
import time
from IPython.display import HTML
import warnings
warnings.filterwarnings("ignore")

#data processing
from sklearn.preprocessing import OneHotEncoder
from sklearn.datasets import make_blobs

#create a custom color map
my_cmap = matplotlib.colors.LinearSegmentedColormap.from_list("", ["red","yellow","green"])
np.random.seed(0)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
