---
title: matplotlib example sample multiple 1 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'sample multiple 1'


Modules used in program: 
* `import numpy as np`

## python sample multiple 1

Python matplotlib example: sample multiple 1

```python
import numpy as np
from figure import Figure

t = np.linspace(0, 2 * np.pi, 1001)
p = np.array([[1, 0], [0, 1], [-1, 0], [1, 0]])

fig = Figure()
fig.create_grid((3, 3))
fig[0, 0].plot(np.cos(t), np.sin(t))
fig[0, 0].plot(0.5 * np.cos(t), np.sin(t))
fig[0, 2].plot(np.cos(t), np.sin(t))
fig[0, 2].plot(0.5 * np.cos(t), np.sin(t))
fig[1, 1].plot(p[:, 0], p[:, 1])
fig[1, 1].plot(0.4 * np.cos(t), 0.2 * np.sin(2 * t) + 0.3)
fig[2, :].plot(np.cos(t), -np.sin(t))
fig[2, :].plot(np.cos(t), 0 * np.sin(t))
fig.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
