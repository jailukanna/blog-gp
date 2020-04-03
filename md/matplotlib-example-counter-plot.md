---
title: matplotlib example counter-plot (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'counter-plot'

Functions in program: 
* `def counter_plot(values, max_count=len(set(values)),`

Modules used in program: 
* `import seaborn as sns`
* `import matplotlib.pyplot as plt`

## python counter-plot

Python matplotlib example: counter-plot

```python
import matplotlib.pyplot as plt
import seaborn as sns
from collections import Counter

def counter_plot(values, max_count=len(set(values)),
                 figsize=(17,5), dpi=80,
                 fontsize=12, rotation=90,
                 xoffset=.05, yoffset=.1):
    c = Counter(values)
    l, v = zip(*c.most_common(max_count))
    i = list(range(len(l)))
    plt.figure(figsize=figsize, dpi=dpi)
    sns.barplot(i, v)
    plt.xticks(i, l, rotation=rotation,
               fontsize=fontsize)
    plt.yticks(fontsize=fontsize)
    plt.ylabel('counts', fontsize=fontsize+5)
    for ii, vv in zip(i, v):
        plt.text(ii-xoffset, vv+yoffset, vv,
                 color='m', rotation=40,
                 fontsize=fontsize)
    plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
