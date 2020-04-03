---
title: matplotlib example plot topk (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'plot topk'

Functions in program: 
* `def add_topk(img1, probs, names):`
* `def plot_topk(probs, names, buf='tmp.png'):`

Modules used in program: 
* `import matplotlib.ticker as mtk`
* `import matplotlib.gridspec as gridspec`
* `import matplotlib.pyplot as plt`
* `import matplotlib`
* `import numpy as np`
* `import io`

## python plot topk

Python matplotlib example: plot topk

```python
import io

import numpy as np
import matplotlib
matplotlib.use('Agg')  # noqa: E402

import matplotlib.pyplot as plt
import matplotlib.gridspec as gridspec
import matplotlib.ticker as mtk

from PIL import Image


def plot_topk(probs, names, buf='tmp.png'):
    k = len(probs)

    probs = np.array(probs)
    names = np.array(names, dtype=str)
    ind = np.argsort(np.array(probs))
    probs = probs[ind]
    names = names[ind]

    fig = plt.figure(figsize=(4, 2))
    gs = gridspec.GridSpec(1, 1)

    ax1 = fig.add_subplot(gs[0, 0])

    ax1.tick_params(
        bottom=False, labelbottom=False, left=False, labelleft=True)
    ax1.tick_params(axis='y', direction='in', pad=-5)

    ax1.barh(names, probs, height=1, alpha=0.4)

    ym = mtk.IndexLocator(1.0, 0)
    ax1.yaxis.set_minor_locator(ym)
    ax1.yaxis.grid(
        which="minor", color='lightgray', linestyle='-', linewidth=1)
    ax1.yaxis.set_ticks_position('none')
    ax1.set_yticklabels(names, ha='left')

    ax1.set_xlim([0, 1.0])
    ax1.set_ylim([-0.5, k - .5])

    ax2 = ax1.twinx()
    ax2.tick_params(right=False)
    ax1.yaxis.set_ticks_position('none')
    ax2.set_ylim(ax1.get_ylim())
    ax2.set_yticks(ax1.get_yticks())
    ax2.set_yticklabels(['{:.4f}'.format(p) for p in probs], ha='right')
    ax2.tick_params(axis='y', direction='in', pad=-5)

    gs.tight_layout(fig, pad=0)
    plt.savefig(buf, dpi=100, format='png')
    plt.close('all')


def add_topk(img1, probs, names):
    buf = io.BytesIO()
    plot_topk(probs, names, buf)
    buf.seek(0)
    img2 = Image.open(buf)
    size2 = (img1.width, int(np.ceil(img2.height*img2.width/img1.width)))
    img2.thumbnail(size2, Image.ANTIALIAS)

    img = Image.new('RGB', (img1.width, img1.height + img2.height))
    img.paste(img1, (0, 0, img1.width, img1.height))
    img.paste(img2, (0, img1.height, img2.width, img1.height + img2.height))
    buf.close()
    return img


if __name__ == '__main__':
    plot_topk([0.6, 0.3, 0.1, 0.05, 0.01, 0.04],
              ['a', 'b', 'c', 'd', 'e', 'f'])


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
