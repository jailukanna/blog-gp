---
title: matplotlib example vector-plot (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'vector-plot'

Functions in program: 
* `def make_grid(s):`

Modules used in program: 
* `import matplotlib.ticker as tick`
* `import matplotlib.colors as colors`
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python vector-plot

Python matplotlib example: vector-plot

```python
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.colors as colors
import matplotlib.ticker as tick


vs = np.array([[1, 0], [0, 1], [1,1]])
vcs = list(map(colors.hsv_to_rgb,
               [(0/3, 3/4, 3/4), (1/3, 3/4, 3/4), (2/3, 3/4, 3/4)]))

m = np.array([[-2,-1], [2, -2]])
vs2 = (m @ vs.T).T

fig, (s1, s2) = plt.subplots(1, 2, figsize=(16, 8))

s1.quiver(
    np.zeros_like(vs[:,0]), np.zeros_like(vs[:,1]), vs[:,0], vs[:,1],
    color=vcs,
    angles="xy", scale_units="xy", scale=1)

s2.quiver(
    np.zeros_like(vs2[:,0]), np.zeros_like(vs2[:,1]), vs2[:,0], vs2[:,1],
    color=vcs,
    angles="xy", scale_units="xy", scale=1)

def make_grid(s):
    s.set_aspect("equal")
    s.set_xlim(-5, 5)
    s.set_ylim(-5, 5)
    # grid
    s.xaxis.set_minor_locator(tick.MultipleLocator(1))
    s.yaxis.set_minor_locator(tick.MultipleLocator(1))
    s.xaxis.set_major_locator(tick.MultipleLocator(5))
    s.yaxis.set_major_locator(tick.MultipleLocator(5))
    s.grid(True, which="major", color="black", linestyle="-")
    s.grid(True, which="minor", color="black", linestyle="--")
    s.set_xlabel("x")
    s.set_ylabel("y")
    pass

make_grid(s1)
make_grid(s2)
s1.set_title("from")
s2.set_title("to: det(A) = 6")

plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
