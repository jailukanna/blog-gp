---
title: matplotlib example plot2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'plot2'

Functions in program: 
* `def ema(y, a):`
* `def savefig(filename):`
* `def newfig(width):`
* `def figsize(scale):`

Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import matplotlib as mpl`
* `import numpy as np`

## python plot2

Python matplotlib example: plot2

```python
import numpy as np
import matplotlib as mpl
mpl.use('pgf')

def figsize(scale):
    fig_width_pt = 469.755                          # Get this from LaTeX using \the\textwidth
    inches_per_pt = 1.0/72.27                       # Convert pt to inch
    golden_mean = (np.sqrt(5.0)-1.0)/2.0            # Aesthetic ratio (you could change this)
    fig_width = fig_width_pt*inches_per_pt*scale    # width in inches
    fig_height = fig_width*golden_mean              # height in inches
    fig_size = [fig_width,fig_height]
    return fig_size

pgf_with_latex = {                      # setup matplotlib to use latex for output
    #"backend": 'ps',
    "pgf.texsystem": "pdflatex",        # change this if using xetex or lautex
    "text.usetex": True,                # use LaTeX to write all text
    "font.family": "serif",
    "font.serif": [],                   # blank entries should cause plots to inherit fonts from the document
    "font.sans-serif": [],
    "font.monospace": [],
    "axes.labelsize": 11.0,               # LaTeX default is 10pt font.
    "text.fontsize": 11.0,
    "legend.fontsize": 11.0,               # Make the legend/label fonts a little smaller
    "xtick.labelsize": 9.0,
    "ytick.labelsize": 9.0,
    "figure.figsize": figsize(0.9),     # default fig size of 0.9 textwidth
    "pgf.preamble": [
        r"\usepackage[utf8x]{inputenc}",    # use utf8 fonts becasue your computer can handle it :)
        r"\usepackage[T1]{fontenc}",        # plots will be generated using this preamble
        ]
    }
mpl.rcParams.update(pgf_with_latex)

import matplotlib.pyplot as plt

# I make my own newfig and savefig functions
def newfig(width):
    plt.clf()
    fig = plt.figure(figsize=figsize(width))
    ax = fig.add_subplot(111)
    return fig, ax

def savefig(filename):
    plt.savefig('{}.pgf'.format(filename), dpi=300, bbox_inches='tight')
    plt.savefig('{}.pdf'.format(filename), dpi=300, bbox_inches='tight')
    plt.savefig('{}.png'.format(filename), dpi=150, bbox_inches='tight')


# Simple plot
fig, ax  = newfig(0.6)

def ema(y, a):
    s = []
    s.append(y[0])
    for t in range(1, len(y)):
        s.append(a * y[t] + (1-a) * s[t-1])
    return np.array(s)
    
y = [0]*200
y.extend([20]*(1000-len(y)))
s = ema(y, 0.01)

ax.margins(y=.05)
ax.margins(x=.05)
ax.legend()
ax.plot(s, linewidth=2)
plt.xlabel(r'One $\alpha$')
plt.ylabel(r'Two $\beta$')
plt.grid(True)

savefig('ema')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
