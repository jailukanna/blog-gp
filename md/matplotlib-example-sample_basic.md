---
title: matplotlib example sample basic (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'sample basic'


Modules used in program: 
* `import numpy as np`

## python sample basic

Python matplotlib example: sample basic

```python
import numpy as np
from figure import Figure

x = np.linspace(0, 2 * np.pi, 1001)
y = np.sin(3 * x)

fig = Figure()
fig[0].plot(x, y)
fig.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
