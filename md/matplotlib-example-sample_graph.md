---
title: matplotlib example sample graph (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'sample graph'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import matplotlib`

## python sample graph

Python matplotlib example: sample graph

```python
import matplotlib
matplotlib.use('agg')
import matplotlib.pyplot as plt

X = [1, 2, 3]
y = [2, 4, 6]

plt.plot(X, y)
plt.title('Title')
plt.xlabel('X')
plt.ylabel('y')
plt.savefig('fig.pdf')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
