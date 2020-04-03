---
title: matplotlib example line graph03 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'line graph03'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python line graph03

Python matplotlib example: line graph03

```python
# -*- coding: utf-8 -*-
import numpy as np
import matplotlib.pyplot as plt

n_groups = 4

data_a = (4, 6, 2, 8)
data_b = (5, 7, 8, 5)
labels = [2014, 2015, 2016, 2017]

fig, ax = plt.subplots()

index = np.arange(n_groups)
bar_width = 0.35

rects1 = ax.barh(index, data_a, bar_width,
                 color='b',
                 label='Group A')

rects2 = ax.barh(index + bar_width, data_b, bar_width,
                 color='r',
                 label='Group B')

ax.set_ylabel('Year')
ax.set_xlabel('Score')
ax.set_title('Grouping Bar Graph')
ax.set_yticks(index + bar_width / 2)
ax.set_yticklabels(labels)
ax.legend()

fig.tight_layout()
plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
