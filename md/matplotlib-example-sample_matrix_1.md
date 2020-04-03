---
title: matplotlib example sample matrix 1 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'sample matrix 1'


Modules used in program: 
* `import numpy as np`

## python sample matrix 1

Python matplotlib example: sample matrix 1

```python
import numpy as np
from figure import Figure

mat = np.random.uniform(size=(10, 10), high=1.0, low=-1.0)

fig = Figure()
fig[0].plot_matrix(mat, vmin=-1.0, vmax=1.0, cmap="bwr", flip_axis=True)

fig.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
