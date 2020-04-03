---
title: matplotlib example ImportPack (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'ImportPack'


Modules used in program: 
* `import os`
* `import gc`
* `import seaborn as sns # Seaborn visualization library`
* `import matplotlib.pyplot as plt`
* `import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)`
* `import numpy as np # linear algebra`

## python ImportPack

Python matplotlib example: ImportPack

```python
import numpy as np # linear algebra
import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)
import matplotlib.pyplot as plt
import seaborn as sns # Seaborn visualization library
sns.set(style="darkgrid")
import gc
import os

%matplotlib inline

train = pd.read_csv('../input/train.csv')
test = pd.read_csv('../input/test.csv')

# Check out the shape of the train and test sets
print('Train:', train.shape)
print('Test:', test.shape)

# Result #
Train: (200000, 202)
Test: (200000, 201)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
