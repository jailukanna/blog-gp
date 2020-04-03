---
title: matplotlib example sample lineplot (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'sample lineplot'

Functions in program: 
* `def plot():`

Modules used in program: 
* `import matplotlib.pyplot as plt`

## python sample lineplot

Python matplotlib example: sample lineplot

```python
import matplotlib.pyplot as plt
%matplotlib inline

def plot():
  # Keep a length of 12
  x_0 = [1.0, 1.2, 1.3, 2.5, 3.0, 3.1] # len=6
  x_1 = [3.1, 2.9, 2.0, 1.0, 1.1, 1.1, 1.0, 1.2, 1.3, 3.0, 3.5, 3.5] #len=18
  x_2 = [3.4, 3.0, 2.0, 1.1, 1.1, 1.2, 1.1, 1.4, 1.2, 4.0, 4.5, 5.0] #len=30
  x_3 = [5.1, 4.5, 4.0, 1.9, 1.1, 1.2, 1.3, 1.0, 1.5, 5.5, 6.7, 7.0] #len=42
  x_4 = [7.1, 6.2, 5.9, 2.9, 2.0, 1.4, 1.3, 1.2, 1.3, 4.0, 4.5, 4.6] #len=54
  x_5 = [4.5, 4.5, 4.4, 3.0, 2.1, 1.1, 1.2, 1.3, 1.3, 4.4, 5.0, 5.1] #len=66
  x_6 = [5.1, 4.9, 4.8, 2.1, 1.3, 1.4, 1.2, 1.1, 1.2, 5.0, 5.2, 5.3] #len=78
  x_plot = x_0 + x_1 + x_2 + x_3 + x_4 + x_5 + x_6

  x_0_ = [1.1, 1.1, 1.4, 2.4, 3.1, 3.2]
  x_1_ = [3.2, 2.7, 1.9, 1.1, 1.2, 1.1, 1.0, 1.1, 1.4, 3.1, 3.5, 3.2]
  x_2_ = [3.1, 3.2, 2.5, 1.3, 1.1, 1.2, 1.1, 1.5, 1.2, 4.1, 4.6, 5.2]
  x_3_ = [5.0, 4.6, 4.0, 2.2, 1.5, 1.2, 1.4, 1.0, 1.9, 6.0, 6.7, 6.9]
  x_4_ = [6.5, 6.1, 6.0]
  x_5_ = []
  x_6_ = []
  x_plot_ = x_0_ + x_1_ + x_2_ + x_3_ + x_4_ + x_5_ + x_6_


  f, axarr = plt.subplots(1, figsize=(10,10))
  axarr.plot(x_plot, linestyle='--', label='Prediction')
  axarr.plot(x_plot_, label='Actual')
  axarr.set_ylabel('Average Bird Density (birds/km2)', fontsize=15)
  axarr.set_xlabel('Time', fontsize=15)
  axarr.set_xticks([])
  axarr.set_xticks([5,17,29,41,53,65,77])
  axarr.set_xticklabels(["13/05", "14/05", "15/05", "16/05",'17/05', '18/05', '19/05'])
  axarr.set_yticks([1,2,3,4,5,6,7])
  axarr.set_yticklabels(['0','30','60','90', '120', '150', '180', '210'])
  axarr.legend(fontsize=11)

  vspan_c = 'b'
  axarr.axvspan(3, 8, facecolor=vspan_c, alpha=0.1)
  axarr.axvspan(15, 20, facecolor=vspan_c, alpha=0.1)
  axarr.axvspan(27, 32, facecolor=vspan_c, alpha=0.1)
  axarr.axvspan(39, 44, facecolor=vspan_c, alpha=0.1)
  axarr.axvspan(51, 56, facecolor=vspan_c, alpha=0.1)
  axarr.axvspan(63, 68, facecolor=vspan_c, alpha=0.1)
  axarr.axvspan(75, 80, facecolor=vspan_c, alpha=0.1)

  plt.text(65.8, 6.5, 'Note : The stripes \nrepresent night', color='blue', fontweight='bold', bbox=dict(boxstyle='round', facecolor='wheat', alpha=0.5))
 
if __name__ == "__main__":
  plot()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
