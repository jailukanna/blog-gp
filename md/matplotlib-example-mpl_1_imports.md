---
title: matplotlib example mpl 1 imports (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'mpl 1 imports'


Modules used in program: 
* `import matplotlib.ticker as mtick`
* `import matplotlib.pyplot as plt`
* `import pandas as pd`
* `import numpy as np`
* `import os`

## python mpl 1 imports

Python matplotlib example: mpl 1 imports

```python
%matplotlib inline

import os
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from matplotlib import font_manager as fm, rcParams
import matplotlib.ticker as mtick

data = pd.read_csv('data/kc_house_data.csv')
df = data.sample(100)

x = df['sqft_living']
y = df['price']

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
