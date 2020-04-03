---
title: matplotlib example eigen-plot (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'eigen-plot'

Functions in program: 
* `def make_grid(s):`
* `def area_plot(s, vs):`

Modules used in program: 
* `import matplotlib.ticker as tick`
* `import matplotlib.colors as colors`
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python eigen-plot

Python matplotlib example: eigen-plot

```python
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.colors as colors
import matplotlib.ticker as tick


vs = np.array([[1, 0], [0, 1]])

m = np.array([[2, 1], [2, 3]])
e, v = np.linalg.eig(m)
d, vi = np.diag(e), np.linalg.inv(v)

vs1 = np.vstack([vs, v.T])
#print(vs1)
vs2 = (vi @ vs1.T).T
vs3 = (d @ vi @ vs1.T).T
vs4 = (v @ d @ vi @ vs1.T).T
#vs4 = (m @ vs.T).T

vcs = list(map(colors.hsv_to_rgb, [
    (0/3, 3/4, 3/4), (1/3, 3/4, 3/4), (2/3, 3/4, 3/4),
    (1/6, 3/4, 3/4), (3/6, 3/4, 3/4), (5/6, 3/4, 3/4)]))

fig, ((s1, s2), (s3, s4)) = plt.subplots(2, 2, figsize=(16, 16))

def area_plot(s, vs):
    s.quiver(
        np.zeros_like(vs[:,0]), np.zeros_like(vs[:,1]), vs[:,0], vs[:,1],
        color=vcs, angles="xy", scale_units="xy", scale=1)
    b = np.array([[0, 0], vs[0], vs[0] + vs[1], vs[1]])
    e = np.array([[0, 0], vs[2], vs[2] + vs[3], vs[3]])
    s.fill(b.T[0], b.T[1], alpha=1/4)
    s.fill(e.T[0], e.T[1], alpha=1/4)
    pass

area_plot(s1, vs1)
area_plot(s2, vs2)
area_plot(s3, vs3)
area_plot(s4, vs4)

def make_grid(s):
    s.set_aspect("equal")
    s.set_xlim(-6, 6)
    s.set_ylim(-6, 6)
    # grid
    s.xaxis.set_minor_locator(tick.MultipleLocator(1))
    s.yaxis.set_minor_locator(tick.MultipleLocator(1))
    s.xaxis.set_major_locator(tick.MultipleLocator(2))
    s.yaxis.set_major_locator(tick.MultipleLocator(2))
    s.grid(True, which="major", color="black", linestyle="-")
    s.grid(True, which="minor", color="black", linestyle="--")
    s.set_xlabel("x")
    s.set_ylabel("y")
    pass

make_grid(s1)
make_grid(s2)
make_grid(s3)
make_grid(s4)
s1.set_title("E")
s2.set_title("V^-1")
s3.set_title("DV^-1")
s4.set_title("A=VDV^-1")

plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
