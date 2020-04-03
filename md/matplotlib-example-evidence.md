---
title: matplotlib example evidence (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'evidence'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import matplotlib`

## python evidence

Python matplotlib example: evidence

```python
"""
Plot evidences for simple and complicated models
================================================

by Andrew Fowlie.
"""

import matplotlib
import numpy as np
import matplotlib.pyplot as plt

from matplotlib.ticker import MaxNLocator
from scipy.stats import norm, uniform


matplotlib.rc('font', **{'size': 22})
matplotlib.rc('text', usetex=False)

plt.xkcd()

fig, ax = plt.subplots(figsize=(12, 8))


# x-axis data

x = np.linspace(-10, 10, 100)

# Good simple model

y = norm.pdf(x, 0, 1)
ax.plot(x, y, 'Brown', linewidth=2, label='Good simple model')
ax.text(-6, 0.3,
        'Good simple model:\nconcentrated\nprobability mass\nat observed data.',
        color='Brown')
plt.fill_between(x, y, 0, color='Brown', alpha=0.5)

# Bad simple model

y = norm.pdf(x, 7, 1)
ax.plot(x, y, 'Olive', linewidth=2, label='Bad simple model')
ax.text(3, 0.4,
       'Bad simple model:\nprobability mass\nwasted here.',
       color='Olive')
plt.fill_between(x, y, 0, color='Olive', alpha=0.5)

# Complicated model

y = uniform.pdf(x, -7, 14)
ax.plot(x, y, 'RoyalBlue', linewidth=2, label='Complicated model')
ax.text(-9, 0.1,
        'OK complicated model:\nspreads probability\nmass thinly.',
        color='RoyalBlue')
plt.fill_between(x, y, 0, color='RoyalBlue', alpha=0.5)

# Appearance

plt.xlabel(r'Data')
plt.ylabel(r'Evidence, Prob(Data | Model)')
plt.title('"Naturalness" or Occam\'s razor')

ax.set_xlim(-10, 10)
ax.set_ylim(0, 0.6)

ax.legend(loc='upper left')
ax.xaxis.set_major_locator(MaxNLocator(1))
plt.setp(ax, xticklabels=['', 'Observed data'], yticklabels=[])

plt.savefig('evidence.pdf')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
