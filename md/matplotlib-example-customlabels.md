---
title: matplotlib example customlabels (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'customlabels'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import matplotlib.patches as mpatches`

## python customlabels

Python matplotlib example: customlabels

```python
import matplotlib.patches as mpatches
import matplotlib.pyplot as plt

red_patch = mpatches.Patch(color='red', label='The red data')
plt.legend(handles=[red_patch])

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
