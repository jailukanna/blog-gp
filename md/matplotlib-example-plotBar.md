---
title: matplotlib example plotBar (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'plotBar'

Functions in program: 
* `def plot(data):`

Modules used in program: 
* `import matplotlib.pyplot as plt`

## python plotBar

Python matplotlib example: plotBar

```python
import matplotlib.pyplot as plt

def plot(data):
	f, axarr = plt.subplots(2,1, sharex=True)
    axarr[0].bar(range(len(data)), data)
    axarr[0].set_title('Data1')
    axarr[0].set_xticks(range(len(data)))   
	axarr[0].set_xticklabels(['Exact','0.1','0.33','0.5','1','3','5','10','100'])
	
    axarr[0].bar(range(len(data)), data)
    axarr[0].set_title('Data2')
    

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
