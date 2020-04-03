---
title: matplotlib example sample multiple 3 2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'sample multiple 3 2'


Modules used in program: 
* `import numpy as np`

## python sample multiple 3 2

Python matplotlib example: sample multiple 3 2

```python
import numpy as np
from figure import Figure

x = np.linspace(0, 2 * np.pi, 1001)

fig = Figure()
fig.set_figsize((8, 8))
fig.create_grid((4, 1), hspace=0.0, height_ratios=(1, 1, 1, 2))

y1 = np.sin(5 * x)
y2 = np.sin(7 * x)
y3 = np.sin(11 * x)
y4 = 0.5 * y1 + 0.3 * y2 + 0.1 * y3
fig[0].plot(x, y1)
fig[1].plot(x, y2)
fig[2].plot(x, y3)
fig[3].plot(x, y4)

for _ in range(4):
    fig[_].grid()

fig.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
