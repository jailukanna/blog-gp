---
title: matplotlib example candlestick (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'candlestick'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import pandas as pd`

## python candlestick

Python matplotlib example: candlestick

```python
"""
# Python Candlestick
## Setup Steps
1. Download .zip from [matplotlib/mpl-finance](https://github.com/matplotlib/mpl-finance).
2. Unzip and put directory as named `mpl_finance` under this script.
3. Run the script.
"""

from random import randint as rand
import pandas as pd
import matplotlib.pyplot as plt
from mpl_finance import mpl_finance

# Generate data
t = 20
quote = []
for i in range(20):
    o = t
    h = t + rand(2, 3)
    l = t - rand(2, 3)
    if rand(0, 1):
        c = t + rand(1, 2)
    else:
        c = t - rand(1, 2)
    t = c
    quote.append((i, o, h, l, c))

quote = pd.DataFrame(quote, columns=['Date', 'Open', 'High', 'Low', 'Close'])
print(quote)

# Plot candlestick
ax = plt.subplot()
mpl_finance.candlestick_ohlc(ax, quote.values, colorup='r', colordown='g')
ax.grid(True)
plt.savefig('./candlestick.png')


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
