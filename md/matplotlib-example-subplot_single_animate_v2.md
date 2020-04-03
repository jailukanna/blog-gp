---
title: matplotlib example subplot single animate v2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'subplot single animate v2'

Functions in program: 
* `def plot_data(fig,axarr,data, IS_IPYTHON):`

Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import matplotlib`

## python subplot single animate v2

Python matplotlib example: subplot single animate v2

```python
import matplotlib
import matplotlib.pyplot as plt
from IPython import display # [for Jupyter]

def plot_data(fig,axarr,data, IS_IPYTHON):
  axarr.cla() # clear axis
  axarr.set_title('Training...')
  axarr.set_xlabel('X-axis')
  axarr.set_ylabel('Y-axis')
  axarr.plot(data)
  
  if IS_IPYTHON:
    display.display(fig) # display the figure once again
    display.clear_output(wait=True) # [for jupyter] wait to clear output of the current running cell when new output is received

  plt.pause(0.1) # just for lulz

if __name__ == "__main__":
  IS_IPYTHON = 'inline' in matplotlib.get_backend()
  print((' - IS_IPYTHON : ', IS_IPYTHON))
    
  data = []
  f,axarr = plt.subplots(1)
  for i in range(10):
      data.append(i+1)
      plot_data(f, axarr, data, IS_IPYTHON)

  if IS_IPYTHON:
    display.display(f) # [for jupyter] to keep the plot from disappearing in the end
  else:
    plt.show()         # [for scripts] to keep the plot from disappearing in the end
  print(('Complete'))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
