---
title: matplotlib example sample multiple 2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'sample multiple 2'


Modules used in program: 
* `import numpy as np`

## python sample multiple 2

Python matplotlib example: sample multiple 2

```python
import numpy as np
from figure import Figure

x = np.linspace(0, 2 * np.pi, 1001)

fig = Figure()
fig.set_figsize((7, 7))
fig.create_grid((3, 3))

fig[:2, :2].create_grid((3, 3))
for _i in range(3):
    for _j in range(3):
        fig[:2, :2][_i, _j].plot(
            np.cos(x) ** (_i * 2 + 1),
            np.sin(x) ** (_j * 2 + 1))
        fig[:2, :2][_i, _j].set_aspect("equal", "datalim")

fig[:2, 2].plot(np.sin(x**2), x)
fig[:2, 2].set_aspect("equal", "datalim")

fig[2, :2].plot(x - np.sin(x), 1 - np.cos(x))
fig[2, :2].set_aspect("equal", "datalim")

y = 3 * x
fig[2, 2].plot(np.cos(y) + y * np.sin(y), np.sin(y) - y * np.cos(y))
fig[2, 2].set_aspect("equal", "datalim")

fig.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
