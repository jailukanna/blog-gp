---
title: matplotlib example Viz 3 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'Viz 3'


Modules used in program: 
* `import matplotlib.pyplot as plt`

## python Viz 3

Python matplotlib example: Viz 3

```python
import matplotlib.pyplot as plt

# Data to plot
labels = 'Python', 'C++', 'Ruby', 'Java'
sizes = [215, 130, 245, 210]
colors = ['gold', 'yellowgreen', 'lightcoral', 'lightskyblue']
explode = (0.1, 0, 0, 0)  # explode 1st slice

# Plot
plt.pie(sizes, explode=explode, labels=labels, colors=colors,
        autopct='%1.1f%%', shadow=True, startangle=140)

plt.axis('equal')
plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
