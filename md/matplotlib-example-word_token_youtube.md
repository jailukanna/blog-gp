---
title: matplotlib example word token youtube (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'word token youtube'


Modules used in program: 
* `import seaborn as sns`
* `import plotly`
* `import plotly.graph_objs as go`
* `import matplotlib`
* `import matplotlib.mlab as mlab`
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import pandas as pd`

## python word token youtube

Python matplotlib example: word token youtube

```python
# import all the packages
import pandas as pd
import numpy as np
from datetime import timedelta, datetime

import matplotlib.pyplot as plt
import matplotlib.mlab as mlab
import matplotlib
plt.style.use('ggplot')
from matplotlib.pyplot import figure

from plotly import __version__
from plotly.offline import download_plotlyjs, init_notebook_mode, plot, iplot
import plotly.graph_objs as go
import plotly

%matplotlib inline
matplotlib.rcParams['figure.figsize'] = (12,8)

from sklearn.tree import DecisionTreeRegressor
from sklearn import tree
from sklearn.ensemble import GradientBoostingRegressor, GradientBoostingClassifier
from sklearn.model_selection import cross_val_score

import seaborn as sns

pd.options.mode.chained_assignment = None

from nltk import word_tokenize
from collections import Counter

words = df_videos['title'].str.lower().str.cat(sep=' ')
word_tokens = word_tokenize(words)

word_counter = Counter(word_tokens)
print('{} different words'.format(len(word_counter)))
word_counter.most_common(538)



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
