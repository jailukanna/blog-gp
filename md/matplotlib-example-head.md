---
title: matplotlib example head (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'head'

Functions in program: 
* `def main():`

Modules used in program: 
* `import os`
* `import matplotlib.pylab as plt`
* `import pandas as pd `
* `import scipy as sp`

## python head

Python matplotlib example: head

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import scipy as sp
import pandas as pd 
import matplotlib.pylab as plt
import os

def main():
    os.system('cls')
    print("Todo:\n",pd.read_csv('datos.csv'))
    print("\n-----------------------")
    datos = pd.read_csv('datos.csv')
    print("Head:\n",datos.head())
    print("\n-----------------------")
    print("Info:\n",datos.info())
    print("\n-----------------------")
    print("Describe:\n",datos.describe())
    t = sp.linspace(0,2, 10)
    plt.plot(t, t*2)
    plt.show()
    

if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
