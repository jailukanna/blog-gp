---
title: matplotlib example show glitches (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'show glitches'

Functions in program: 
* `def load(name):`
* `def glitches_per_second(a):`
* `def normalize(a):`
* `def smooth(a):`
* `def read_channel(name):`

Modules used in program: 
* `import scipy.signal`
* `import scipy.io.wavfile`
* `import matplotlib.ticker`
* `import matplotlib`
* `import pylab`
* `import numpy`
* `import sys`
* `import os.path`

## python show glitches

Python matplotlib example: show glitches

```python
#!env python3
import os.path
import sys
import numpy
import pylab
import matplotlib
import matplotlib.ticker
import scipy.io.wavfile
import scipy.signal

rate = 44100

def read_channel(name):
    return scipy.io.wavfile.read(name)[1][:,1]

def smooth(a):
    return scipy.signal.savgol_filter(a, 51, 3)

def normalize(a):
    a = numpy.trim_zeros(a)
    a = smooth(a)
    return a

def glitches_per_second(a):
    da = numpy.gradient(a, a[1]-a[0])
    da = smooth(da)

    glitches = numpy.zeros_like(da)
    glitches[da == 0] = 1

    glitch_per_sec = numpy.zeros(len(glitches)//rate+1)
    for n, k in enumerate(range(0,len(glitches),rate)):
        glitch_per_sec[n] = numpy.sum(glitches[k:k+rate])

    non_zero_gps = glitch_per_sec != 0

    x = numpy.arange(len(glitch_per_sec))[non_zero_gps]
    y = glitch_per_sec[non_zero_gps]

    return x, y

def load(name):
    a = read_channel(name)
    a = normalize(a)
    return glitches_per_second(a)

matplotlib.use('Agg')

names = sys.argv[1:]

f, plots = pylab.subplots(nrows=len(names), ncols=1, sharex=True, sharey=True)
if len(names) == 1:
    plots = [plots]

for n, name in enumerate(names):
    x, y = load(name)

    xticks = numpy.arange(0,x[-1],60*5)
    yticks = numpy.arange(0,rate+1,rate/2)

    p = plots[n]
    p.plot(x, y, '.')
    p.grid(True)
    p.set_xticks(xticks)
    p.set_yticks(yticks)
    p.xaxis.set_major_formatter(
        matplotlib.ticker.FuncFormatter(lambda v, p: '%dm' % (v // 60)))
    p.set_title(r'$\bf{%s}$' % os.path.splitext(
        os.path.basename(name))[0])

pylab.gcf().tight_layout(pad=2.5)
pylab.gcf().set_size_inches(10, 5.5)

pylab.savefig('glitches.png')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
