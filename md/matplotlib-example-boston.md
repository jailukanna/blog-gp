---
title: matplotlib example boston (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'boston'


Modules used in program: 
* `import seaborn as sns`
* `import statsmodels.api as sm`
* `import sklearn`
* `import matplotlib.pyplot as plt`
* `import scipy.stats as stats`
* `import pandas as pd`
* `import numpy as np`

## python boston

Python matplotlib example: boston

```python
%matplotlib inline 

import numpy as np
import pandas as pd
import scipy.stats as stats
import matplotlib.pyplot as plt
import sklearn
import statsmodels.api as sm

import seaborn as sns
sns.set_style("whitegrid")
sns.set_context("poster")

# special matplotlib argument for improved plots
from matplotlib import rcParams

from sklearn.datasets import load_boston
boston = load_boston()

bos = pd.DataFrame(boston.data)

bos.columns = boston.feature_names
bos['PRICE'] = boston.target

X = bos.drop('PRICE', axis = 1)
Y = bos['PRICE']

X_train, X_test, Y_train, Y_test =\
sklearn.cross_validation.train_test_split(X, Y, test_size = 0.33, random_state = 5)

from sklearn.linear_model import LinearRegression

lm = LinearRegression()
lm.fit(X_train, Y_train)

Y_pred = lm.predict(X_test)

plt.scatter(Y_test, Y_pred)
plt.xlabel("Prices: $Y_i$")
plt.ylabel("Predicted prices: $\hat{Y}_i$")
plt.title("Prices vs Predicted prices: $Y_i$ vs $\hat{Y}_i$")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
