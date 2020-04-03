---
title: matplotlib example fc7627756370-2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'fc7627756370-2'

Functions in program: 
* `def load_ipython_extension(ipython: InteractiveShell):`

Modules used in program: 
* `import seaborn as sns`
* `import matplotlib.pyplot as plt`
* `import pandas as pd`
* `import numpy as np`

## python fc7627756370-2

Python matplotlib example: fc7627756370-2

```python
# ipython-utils/researchenv/__init__.py

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from IPython.core.interactiveshell import InteractiveShell

def load_ipython_extension(ipython: InteractiveShell):
    print('%matplotlib inline')
    print('import numpy as np')
    print('import pandas as pd')
    print('import matplotlib.pyplot as plt')
    print('import seaborn as sns')
    ipython.enable_matplotlib(gui='inline')
    ipython.push({
        'np': np,
        'pd': pd,
        'plt': plt,
        'sns': sns,
    })

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
