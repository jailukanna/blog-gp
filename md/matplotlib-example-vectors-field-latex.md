---
title: matplotlib example vectors-field-latex (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'vectors-field-latex'


Modules used in program: 
* `import matplotlib`

## python vectors-field-latex

Python matplotlib example: vectors-field-latex

```python
# import useful modules
import matplotlib
from numpy import *
from pylab import *

# use LaTeX, choose nice some looking fonts and tweak some settings
matplotlib.rc('font', family='serif')
matplotlib.rc('font', size=16)
matplotlib.rc('legend', fontsize=16)
matplotlib.rc('legend', numpoints=1)
matplotlib.rc('legend', handlelength=1.5)
matplotlib.rc('legend', frameon=False)
matplotlib.rc('xtick.major', pad=7)
matplotlib.rc('xtick.minor', pad=7)
matplotlib.rc('text', usetex=True)
matplotlib.rc('text.latex',
              preamble=[r'\usepackage[T1]{fontenc}',
                        r'\usepackage{amsmath}',
                        r'\usepackage{txfonts}',
                        r'\usepackage{textcomp}'])

close('all')
figure(figsize=(6, 4.5))

# generate grid
x=linspace(-2, 2, 32)
y=linspace(-1.5, 1.5, 24)
x, y=meshgrid(x, y)
# calculate vector field
vx=-y/sqrt(x**2+y**2)*exp(-(x**2+y**2))
vy= x/sqrt(x**2+y**2)*exp(-(x**2+y**2))
# plot vecor field
quiver(x, y, vx, vy, pivot='middle', headwidth=4, headlength=6)
xlabel('$x$')
ylabel('$y$')
axis('image')
#show()
savefig('visualization_vector_fields_1.png')


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
