---
title: matplotlib example bug (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'bug'

Functions in program: 
* `def plot_date_problem():`
* `def get_data(start=datetime.datetime(2012,01,01)):`

Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import matplotlib; matplotlib.use('cairo')`
* `import random`
* `import datetime`

## python bug

Python matplotlib example: bug

```python
import datetime
import random
import matplotlib; matplotlib.use('cairo')
import matplotlib.pyplot as plt
from matplotlib.dates import DayLocator, MonthLocator, DateFormatter, date2num

"""
Minimal(ish) repro for the x-axis label alignment bug

Collecting info suggested here: http://matplotlib.org/faq/troubleshooting_faq.html
Mac OSX 10.8.2
Matplotlib 1.3.x, installed from ScipySuperpack: https://github.com/fonnesbeck/ScipySuperpack
Matplotlibrc: https://gist.github.com/huyng/816622
Output of $ python bug.py --verbose-debug

$HOME=/Users/jsundram
CONFIGDIR=/Users/jsundram/.matplotlib
matplotlib data path /Library/Python/2.7/site-packages/matplotlib-1.3.x-py2.7-macosx-10.8-intel.egg/matplotlib/mpl-data
loaded rc file /Users/jsundram/.matplotlib/matplotlibrc
matplotlib version 1.3.x
verbose.level debug
interactive is True
platform is darwin
loaded modules: <dictionary-keyiterator object at 0x104355c00>
Using fontManager instance from /Users/jsundram/.matplotlib/fontList.cache
backend cairo version 1.10.0
findfont: Matching :family=monospace:style=normal:variant=normal:weight=normal:stretch=normal:size=medium to Andale Mono (/Library/Fonts/Andale Mono.ttf) with score of 0.000000
findfont: Matching :family=monospace:style=normal:variant=normal:weight=normal:stretch=normal:size=x-large to Andale Mono (/Library/Fonts/Andale Mono.ttf) with score of 0.000000
"""

def get_data(start=datetime.datetime(2012,01,01)):
    """ Make up some data """
    random.seed(0) # the same random series every time is better
    dates, data = [start], [1]
    length, mu, sigma = 365, 1, .1
    increment = float(mu) / length
    for i in range(length):
        dates.append(start + datetime.timedelta(i))
        data.append(random.gauss(mu, sigma))
        mu += increment # upward trending
    
    return dates, data

def plot_date_problem():
    """ It looks pretty clear to me that this is a problem with cairo and plot_date.
        Using MacOSX backend, there's no problem. Using ax.plot, there's no problem.
    """
    dates, data = get_data()
    mpl_dates = map(date2num, dates)
    
    fig = plt.figure()
    ax = fig.add_subplot(111)
    ax.plot_date(mpl_dates, data, '-', label='my data')

    plt.ylim(ymin=0) # Start at 0. why is this in plt and not fig or ax?
    ax.xaxis.set_major_locator(MonthLocator())
    ax.xaxis.set_major_formatter(DateFormatter("%b"))
    ax.autoscale_view()
    
    # Show lines making it easier to see the alignment problem
    line_top = plt.axhline(y=-.03, color='k', linewidth=.5)
    line_top.set_clip_on(False) # make it visible below the axis
    line_bot = plt.axhline(y=-.06, color='k', linewidth=.5)
    line_bot.set_clip_on(False) # make it visible below the axis
    plt.title("Matplotlib %s" % matplotlib.__version__)
    
    fig.savefig('bug.png', dpi=300)


if __name__ == '__main__':
    plot_date_problem()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
