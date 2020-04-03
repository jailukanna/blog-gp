---
title: matplotlib example boxplots (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'boxplots'

Functions in program: 
* `def make_boxplot(ax, data, title):`

Modules used in program: 
* `import numpy as np`

## python boxplots

Python matplotlib example: boxplots

```python
import numpy as np
figsize = (10, 4) # set the fgsize for both the box plots and histograms
# make boxplots

# notch shape box plot

def make_boxplot(ax, data, title):
    labels = [d.name for d in data]
    ax.boxplot(data,
                notch=True,  # notch shape
                vert=True,   # vertical box aligmnent
                labels=labels,
                patch_artist=False,
                showbox=True,
                showfliers=False)   # fill with color
    ax.set_title(title)
    
    # jittered data points, see <https://stackoverflow.com/questions/23519135>
    # seaborn does this better
    for i, d in enumerate(data):
        y = data[i]
        x = np.random.normal(i+1, 0.02, len(y))
        ax.plot(x, y, 'r.', alpha=0.6)
    
fig, ax = plt.subplots(ncols=3, figsize=figsize)

make_boxplot(ax[0], (df_deduped["asym_m"], df_deduped["asym_a"]), "asym")
make_boxplot(ax[1], (df_deduped["plates_m"], df_deduped["plates_a"]), "plates per m")
make_boxplot(ax[2], (df_deduped["hetp_m"], df_deduped["hetp_a"]), "hetp")
plt.tight_layout()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
