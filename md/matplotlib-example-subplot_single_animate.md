---
title: matplotlib example subplot single animate (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'subplot single animate'


Modules used in program: 
* `import matplotlib.pyplot as plt`

## python subplot single animate

Python matplotlib example: subplot single animate

```python
from IPython import display
import matplotlib.pyplot as plt

f, axarr = plt.subplots(1)
w,h = 20, 20
frame1 = plt.gca()
frame1.set_xlim(0,w)
frame1.set_ylim(0,h)
cmap = plt.cm.rainbow
color_me = np.random.rand(w)

for i in range(w):
	axarr.scatter(i,i, c = cmap(color_me[i]))
	display.display(plt.gcf())
	display.clear_output(wait=True)
	plt.pause(0.2)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
