---
title: matplotlib example sample wheel (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'sample wheel'


Modules used in program: 
* `import numpy as np`

## python sample wheel

Python matplotlib example: sample wheel

```python
import numpy as np
from figure import Figure

t = np.linspace(0, 2 * np.pi, 1001)
x = np.cos(3 * t)
y = np.sin(4 * t)

fig = Figure()
fig.set_figsize((6, 6))
fig[0].plot(x, y)
fig[0].add_zoom_func()
fig.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
