---
title: matplotlib example pydata boilerplate (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'pydata boilerplate'


Modules used in program: 
* `import ibis`
* `import matplotlib.pyplot as plt`
* `import matplotlib`
* `import pandas as pd`
* `import numpy as np`

## python pydata boilerplate

Python matplotlib example: pydata boilerplate

```python
%autosave 30
%matplotlib inline

import numpy as np
import pandas as pd
import matplotlib
import matplotlib.pyplot as plt
matplotlib.style.use('ggplot')
matplotlib.style.use('dark_background') # 暗い背景用

# from datetime import datetime
# from datetime import date
# from dateutil.parser import parse

# from statsmodels.tsa import arima_model
# import statsmodels.tsa.stattools as stattools
# import statsmodels.graphics.tsaplots as tsaplots
# import statsmodels.api as sm

# from math import sqrt, pow
# from pandas.tools.plotting import scatter_matrix

import ibis
ibis.options.interactive = True
ibis.options.sql.default_limit = 10000


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
