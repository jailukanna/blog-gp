---
title: matplotlib example foo (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'foo'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import pandas as pd`
* `import matplotlib`

## python foo

Python matplotlib example: foo

```python
# see https://matplotlib.org/users/pgf.html

# set matplotlib backend (before actually importing matplotlib)
import matplotlib
matplotlib.use('pgf')
pgf_with_rc_fonts = {
    "font.family": "serif",
    "font.serif": [],                    # use latex default serif font
    "font.sans-serif": ["DejaVu Sans"],  # use a specific sans-serif font
    "text.usetex": True,
    "pgf.rcfonts": False,
}
matplotlib.rcParams.update(pgf_with_rc_fonts)

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
# generate data
test_seq = np.array([0, 0, 0, 1, 2, 2, 3, 1, 1])
pred_seq = np.array([0, 1, 1, 0, 2, 2, 2, 1, 1])
index = pd.DatetimeIndex(start='2017-08-12 06:00:00', freq='10min', periods=test_seq.shape[0])
df = pd.DataFrame(data={'test': test_seq, 'pred': pred_seq}, index=index)

# create figure
fig, ax = plt.subplots()
df.plot(ax=ax, drawstyle="steps-post")
ax.axes.yaxis.set_ticks([i for i in range(df.max().max()+1)]);
ax.axes.set_xlabel('Time')
ax.axes.set_ylabel('Occupied charging stations')

# save figure
fig.savefig('foo.pgf')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
