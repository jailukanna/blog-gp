---
title: matplotlib example Viz 1 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'Viz 1'


Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python Viz 1

Python matplotlib example: Viz 1

```python
import matplotlib.pyplot as plt
import numpy as np

t = np.arange(0.0, 2.0, 0.01)
s = 1 + np.sin(2 * np.pi * t)

fig, ax = plt.subplots()
ax.plot(t, s)

ax.set(xlabel = 'time (s)', ylabel = 'voltage (mV)', title = 'Simple Line Plot')
ax.grid()

plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
