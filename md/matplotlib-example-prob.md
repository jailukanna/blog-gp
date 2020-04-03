---
title: matplotlib example prob (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'prob'


Modules used in program: 
* `import scipy.stats as stats`
* `import matplotlib.mlab as mlab`
* `import matplotlib.pyplot as plt `
* `import matplotlib`
* `import collections`

## python prob

Python matplotlib example: prob

```python
import collections
import matplotlib
matplotlib.use('TkAgg')
import matplotlib.pyplot as plt 
import matplotlib.mlab as mlab
import scipy.stats as stats

data = [1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 3, 4, 4, 4, 4, 5, 6, 6, 6, 7, 7, 7, 7, 7, 7, 7, 7, 8, 8, 9, 9]

c = collections.Counter(data)
count_sum = sum(c.values())

for k, v in c.items():
	print('The relative frequency of number ' + str(k) + ' is ' + str(float(v) / count_sum) + '. The absolute frequency is ' + str(v) + ". ")

plt.figure(1)
plt.boxplot(data)
plt.savefig('boxplot.png')

plt.figure(2)
plt.hist(data, histtype='bar')
plt.savefig('histogram.png')

plt.figure(3)
prob_plot = stats.probplot(data, dist='norm', plot=plt)
plt.savefig('probability_plot.png')


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
