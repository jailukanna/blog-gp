---
title: matplotlib example sample matrix 2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'sample matrix 2'


Modules used in program: 
* `import pandas as pd`
* `import numpy as np`

## python sample matrix 2

Python matplotlib example: sample matrix 2

```python
import numpy as np
import pandas as pd
from figure import Figure

mat = np.random.randint(0, 100, size=(10, 10))
df = pd.DataFrame(
    mat, columns=list('ABCDEFGHIJ'), index=list('abcdefghij'))

fig = Figure()
fig[0].plot_dataframe(df, vmin=0, vmax=100, cmap="magma")

fig.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
