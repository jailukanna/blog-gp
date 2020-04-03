---
title: matplotlib example pyplot (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'pyplot'

Functions in program: 
* `def set_plt_style(n):`

## python pyplot

Python matplotlib example: pyplot

```python
def set_plt_style(n):
    colors = sns.husl_palette(n)
    markers = ['o', 'v', '^', '<', '>', '8', 's', 'p', '*', 'h', 'H', 'D', 'd'] * (int(n / 13) + 1)
    linestyles = ['-', ':', '-.', '--', ':', '-.', '--'] * (int(n / 7) + 1)
    return({'color': colors,
            'marker': markers,
            'linestyle': linestyles})


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
