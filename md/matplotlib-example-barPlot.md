---
title: matplotlib example barPlot (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'barPlot'


Modules used in program: 
* `import matplotlib.pyplot as plt`

## python barPlot

Python matplotlib example: barPlot

```python
import matplotlib.pyplot as plt

if __name__ == "__main__":
    errors = [0.338, 0.325, 0.305, 0.272, 0.267]
    plt.bar(range(len(errors)), errors)
    plt.xticks(range(len(errors)), ["5", "10", "30", "50",'100'])
    plt.xlabel('Iterations (T)')
    plt.ylabel('Test Error')
    plt.ylim([0,0.6])
    plt.xticks(rotation=90)
    
    for i, error in enumerate(errors):
        plt.text(i-0.25, error+0.01 , str(error), color='blue', fontweight='bold')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
