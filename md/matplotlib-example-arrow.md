---
title: matplotlib example arrow (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'arrow'


Modules used in program: 
* `import matplotlib.pyplot as plt`

## python arrow

Python matplotlib example: arrow

```python
import matplotlib.pyplot as plt
from matplotlib.patches import Arrow

plt.figure(1, figsize=(3,3))
ax = plt.gca()

ax.annotate("",
            xy=(0.2, 0.2), xycoords='data',
            xytext=(0.8, 0.8), textcoords='data',
            arrowprops=dict(arrowstyle="->",
                            connectionstyle="arc3"), 
            )

plt.arrow(0,0,0.5,0.7, shape='full', lw=1, length_includes_head=True, head_width=.02)

plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
