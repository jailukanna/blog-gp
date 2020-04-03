---
title: matplotlib example matplotlib setup (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'matplotlib setup'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import matplotlib`

## python matplotlib setup

Python matplotlib example: matplotlib setup

```python
import matplotlib
import matplotlib.pyplot as plt

# Avoid Type2 fonts
matplotlib.rcParams['pdf.fonttype'] = 42
matplotlib.rcParams['ps.fonttype'] = 42

# Plots looking nice
plt.style.use('ggplot')  # R-like plots

#Save nice plots
FIGSAVE_KWARGS = dict(bbox_inches='tight')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
