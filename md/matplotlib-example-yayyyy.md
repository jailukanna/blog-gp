---
title: matplotlib example yayyyy (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'yayyyy'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import matplotlib.dates as mdates`
* `import matplotlib.cbook as cbook`

## python yayyyy

Python matplotlib example: yayyyy

```python
import matplotlib.cbook as cbook
import matplotlib.dates as mdates
import numpy as np
import matplotlib.pyplot as plt

with cbook.get_sample_data('goog.npz') as datafile:
    r = np.load(datafile)['price_data'].view(np.recarray)

# Matplotlib prefers datetime instead of np.datetime64.
date = r.date.astype('O')
fig, ax = plt.subplots()
ax.plot(date, r.close)
ax.set_title('Default date handling can cause overlapping labels')

###############################################################################
# Another annoyance is that if you hover the mouse over the window and
# look in the lower right corner of the matplotlib toolbar
# (:ref:`navigation-toolbar`) at the x and y coordinates, you see that
# the x locations are formatted the same way the tick labels are, e.g.,
# "Dec 2004".
#
# What we'd like is for the location in the toolbar to have
# a higher degree of precision, e.g., giving us the exact date out mouse is
# hovering over.  To fix the first problem, we can use
# :func:`matplotlib.figure.Figure.autofmt_xdate` and to fix the second
# problem we can use the ``ax.fmt_xdata`` attribute which can be set to
# any function that takes a scalar and returns a string.  matplotlib has
# a number of date formatters built in, so we'll use one of those.

fig, ax = plt.subplots()
ax.plot(date, r.close)

# rotate and align the tick labels so they look better
fig.autofmt_xdate()

# use a more precise date string for the x axis locations in the
# toolbar
ax.fmt_xdata = mdates.DateFormatter('%Y-%m-%d')
ax.set_title('fig.autofmt_xdate fixes the labels')

###############################################################################
# Now when you hover your mouse over the plotted data, you'll see date
# format strings like 2004-12-01 in the toolbar.

plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
