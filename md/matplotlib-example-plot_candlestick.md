---
title: matplotlib example plot candlestick (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'plot candlestick'

Functions in program: 
* `def plot_candlestick(df, keys=('open', 'high', 'low', 'close'),`

Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python plot candlestick

Python matplotlib example: plot candlestick

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import matplotlib.pyplot as plt

# future-compatibility with Panda 0.25
from pandas.plotting import register_matplotlib_converters
register_matplotlib_converters()

import numpy as np

from matplotlib.ticker import FormatStrFormatter

try:
    from mpl_finance import candlestick_ohlc
except ImportError:
    from matplotlib.finance import candlestick_ohlc
    
def plot_candlestick(df, keys=('open', 'high', 'low', 'close'),
                     colorup='#00EE00', colordown='#FF0000',
                     colorwick='#808080', linewidth=1.3, alpha=1,
                     ax=None, figsize=None, savefig=None, show=True):

    up_open = np.where(df[keys[0]] < df[keys[3]], df[keys[0]], np.nan)
    up_close = np.where(df[keys[0]] < df[keys[3]], df[keys[3]], np.nan)
    down_open = np.where(df[keys[0]] > df[keys[3]], df[keys[0]], np.nan)
    down_close = np.where(df[keys[0]] > df[keys[3]], df[keys[3]], np.nan)

    same_open = np.where(df[keys[0]] == df[keys[3]], df[keys[0]], np.nan)
    same_close = np.where(df[keys[0]] == df[keys[3]],
                          df[keys[3]]-0.0005, np.nan)
    samecolor = colorwick if colorwick is not None else '#808080'

    fig = None
    if ax is None:
        fig, ax = plt.subplots(figsize=figsize)

    # plot candles
    if colorwick is not None:
        ax.vlines(df.index.values, df[keys[2]], df[keys[1]],
                  color=colorwick, linewidth=linewidth/4, alpha=alpha)

    ax.vlines(df.index.values, up_open, up_close,
              linewidth=linewidth, color=colorup, alpha=alpha)
    ax.vlines(df.index.values, down_open, down_close,
              linewidth=linewidth, color=colordown, alpha=alpha)
    ax.vlines(df.index.values, same_open, same_close, linewidth=linewidth *
              1.4, color=samecolor, alpha=min([1, alpha*2]))

    ax.yaxis.set_major_formatter(FormatStrFormatter('%.2f'))

    if fig:
        fig.autofmt_xdate()

    try:
        plt.subplots_adjust(hspace=0, bottom=0, top=1)
    except Exception:
        pass

    try:
        fig.tight_layout()
    except Exception:
        pass

    if savefig:
        if isinstance(savefig, dict):
            plt.savefig(**savefig)
        else:
            plt.savefig(savefig)

    if show and fig:
        plt.show(fig)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
