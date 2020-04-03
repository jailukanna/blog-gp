---
title: matplotlib example sample seaborn (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'sample seaborn'


Modules used in program: 
* `import seaborn as sns`
* `import pandas as pd`
* `import numpy as np`

## python sample seaborn

Python matplotlib example: sample seaborn

```python
import numpy as np
import pandas as pd
import seaborn as sns
from figure import Figure

mat = np.random.randint(0, 100, size=(10, 10))
df = pd.DataFrame(
    mat, columns=list('ABCDEFGHIJ'), index=list('abcdefghij'))

fig = Figure()
fig.set_figsize((8, 4))
fig.create_grid((1, 2))
sns.heatmap(df, vmin=0, vmax=100, cmap="magma", ax=fig[0])
fig[1].plot(mat)

fig.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
