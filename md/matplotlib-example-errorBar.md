---
title: matplotlib example errorBar (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'errorBar'

Functions in program: 
* `def plot1(X):`
* `def getStats(X):`

Modules used in program: 
* `import matplotlib.pyplot as plt`

## python errorBar

Python matplotlib example: errorBar

```python
import matplotlib.pyplot as plt

def getStats(X):
    x = np.arange(len(X))
    y = np.mean(X, axis=1)
    yerr = np.std(X, axis=1)
    return x,y,yerr

def plot1(X):
    x, y, yerr = getStats(X)
    plt.errorbar(x, y, yerr, label='', marker='s')
  
    plt.legend(loc = 'upper right') # plt.legend(fancybox=True, shadow=True, fontsize=8)
    plt.xticks([])
    plt.ylim([-0.5, 0.5])
    plt.xaxis.set_tick_params(labelsize=20)
    plt.grid(True)
    plt.title('Title', fontsize=10);    
    # plt.xscale('log', basex=2)
    
    
if __name__ == "__main__":
  X = [[], [], ... , [], 
  plot1(X)
  
  

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
