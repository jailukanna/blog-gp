---
title: matplotlib example sample multiple 3 1 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'sample multiple 3 1'


Modules used in program: 
* `import numpy as np`

## python sample multiple 3 1

Python matplotlib example: sample multiple 3 1

```python
import numpy as np
from figure import Figure

n = 8
x = np.linspace(0, 2 * np.pi, 1001)

fig = Figure()
fig.set_figsize((8, 8))
fig.create_grid((n, n), hspace=0.0, wspace=0.0)
for _i in range(n):
    for _j in range(n):
        fig[_i, _j].plot(np.cos((_i + 1) * x), np.sin((_j + 1) * x))
        fig[_i, _j].set_xlim([-1.1, 1.1])
        fig[_i, _j].set_ylim([-1.1, 1.1])
        fig[_i, _j].set_xticks([])
        fig[_i, _j].set_yticks([])
        fig[_i, _j].set_aspect("equal", "datalim")

fig.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
