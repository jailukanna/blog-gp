---
title: matplotlib example fft (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'fft'

Functions in program: 
* `def savefig(filename):`
* `def figsize(scale):`

Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python fft

Python matplotlib example: fft

```python
import numpy as np
from scipy import fftpack
import matplotlib.pyplot as plt
from matplotlib import rcParams
from matplotlib import use
#import matplotlib as mpl
use('pgf')


def figsize(scale):
    fig_width_pt = 469.755                          # Get this from LaTeX using \the\textwidth
    inches_per_pt = 1.0/72.27                       # Convert pt to inch
    golden_mean = (np.sqrt(5.0)-1.0)/2.0            # Aesthetic ratio (you could change this)
    fig_width = fig_width_pt*inches_per_pt*scale    # width in inches
    fig_height = fig_width*golden_mean              # height in inches
    fig_size = [fig_width,fig_height]
    return fig_size

#rcParams['text.usetex'] = True
#rcParams['font.size'] = 11.0
#rcParams['font.family'] = 'lmodern'
#rcParams['text.latex.unicode'] = True
#rcParams['text.latex.preamble'] = [r'\boldmath']

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
rcParams.update(pgf_with_latex)

width = 2.0
freq = 0.5
freq2 = 0.025

t = np.linspace(-10, 10, 251)   # linearly space time array
#g = np.exp(-np.abs(t)/width) * np.sin(2.0*np.pi*freq*t) * np.exp(2.5*np.pi*freq2*t)
g = np.sin(2.0*np.pi*freq*t)

dt = t[1]-t[0]       # increment between times in time array

G = fftpack.fft(g)   # FFT of g
f = fftpack.fftfreq(g.size, d=dt) # frequenies f[i] of g[i]
f = fftpack.fftshift(f)     # shift frequencies from min to max
G = fftpack.fftshift(G)     # shift G order to coorespond to f

#fig, ax = plt.subplots(nrows=1, ncols=1)
fig = plt.figure(1, figsize=figsize(width), frameon=True)

ax1 = fig.add_subplot(211)
ax1.plot(t, g, linewidth=2)
ax1.set_xlabel(r'$t$')
ax1.set_ylabel(r'$g(t)$')
ax1.grid(True)

ax2 = fig.add_subplot(212)
ax2.plot(f, np.real(G), color='dodgerblue', label='real part', linewidth=2)
ax2.plot(f, np.imag(G), color='coral', label='imaginary part', linewidth=2)
ax2.legend()
ax2.set_xlabel(r'$f$')
ax2.set_ylabel(r'$G(f)$')
ax2.grid(True)

def savefig(filename):
    plt.savefig('{}.pgf'.format(filename), dpi=300, bbox_inches='tight')
    plt.savefig('{}.pdf'.format(filename), dpi=300, bbox_inches='tight')
    plt.savefig('{}.png'.format(filename), dpi=150, bbox_inches='tight')


#fig.savefig('fft.png', dpi=100, bbox_inches='tight')
savefig('fft')

plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
