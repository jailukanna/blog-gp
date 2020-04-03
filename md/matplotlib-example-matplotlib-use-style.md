---
title: matplotlib example matplotlib-use-style (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'matplotlib-use-style'

Functions in program: 
* `def hist_and_lines():`

## python matplotlib-use-style

Python matplotlib example: matplotlib-use-style

```python
def hist_and_lines():
    np.random.seed(0)
    fig, ax = plt.subplots(1, 2, figsize=(11, 4))
    ax[0].hist(np.random.randn(1000))
    for i in range(3):
        ax[1].plot(np.random.rand(10))
    ax[1].legend(['a', 'b', 'c'], loc='lower left')
    
with plt.style.context('bmh'):
    hist_and_lines()
    
with plt.style.context(['seaborn']):
    hist_and_lines()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
