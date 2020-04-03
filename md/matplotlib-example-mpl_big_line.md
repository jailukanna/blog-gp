---
title: matplotlib example mpl big line (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'mpl big line'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import matplotlib as mpl`
* `import numpy as np`

## python mpl big line

Python matplotlib example: mpl big line

```python
import numpy as np
import matplotlib as mpl
import matplotlib.pyplot as plt

# Creates big x and y data:
N = 10**7
t = np.linspace(0,1,N)
x = np.random.normal(size=N)

# Create a plot:
fig = plt.figure()
ax = plt.subplot(111)

# Create a "big" Line instance:
l = mpl.lines.Line2D(t,x)
l.set_visible(False)

# Interactive panning and zooming is pretty responsive

ax.add_line(l)

# Now interactive panning and zooming is very slowdowned

l.remove()

# Interactive panning and zooming is pretty responsive again

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
