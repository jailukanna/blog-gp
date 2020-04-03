---
title: matplotlib example area-line-plot (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'area-line-plot'

Functions in program: 
* `def make_grid(s):`
* `def line_plots(rs):`

Modules used in program: 
* `import matplotlib.ticker as tick`
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python area-line-plot

Python matplotlib example: area-line-plot

```python
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.ticker as tick

fig, (s1, s2) = plt.subplots(1, 2, figsize=(16, 8))

thetas = np.linspace(0, 2*np.pi, 64)
rs = np.ones_like(thetas)
def line_plots(rs):
    xs = np.array([r * np.cos(theta) for r, theta in zip(rs, thetas)])
    ys = np.array([r * np.sin(theta) for r, theta in zip(rs, thetas)])

    s1.plot(xs, ys)
    s2.plot(rs, thetas)
    pass
line_plots(rs * 1/2)
line_plots(rs * 1)
line_plots(rs * 2)
line_plots(rs * 4)
line_plots(rs * 8)

s1.fill([-10, -10, 10, 10], [-10, 10, 10, -10], alpha=0.25)
s2.fill([0, 0, 10, 10], [0, 2*np.pi, 2*np.pi, 0], alpha=0.25)

def make_grid(s):
    s.set_aspect("equal")
    s.set_xlim(-10, 10)
    s.set_ylim(-10, 10)
    # grid
    s.xaxis.set_minor_locator(tick.MultipleLocator(1))
    s.yaxis.set_minor_locator(tick.MultipleLocator(1))
    s.xaxis.set_major_locator(tick.MultipleLocator(2))
    s.yaxis.set_major_locator(tick.MultipleLocator(2))
    s.grid(True, which="major", color="black", linestyle="-")
    s.grid(True, which="minor", color="black", linestyle="--")
    pass

make_grid(s1)
make_grid(s2)
s1.set_xlabel("x")
s1.set_ylabel("y")
s1.set_title("(x,y) space")

s2.set_xlabel("r")
s2.set_ylabel("θ")
s2.set_title("(r,θ) space")

plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
