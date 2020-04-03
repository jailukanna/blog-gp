---
title: matplotlib example Plots1 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'Plots1'


Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import math`

## python Plots1

Python matplotlib example: Plots1

```python
import math
import matplotlib.pyplot as plt
import numpy as np


#plot sine and cosine

X = np.linspace(0,2*np.pi,100)

Ysin = np.sin(X)
Ycos = np.cos(X)

#plot in red and green
plt.plot(X,Ysin,'r')
plt.plot(X,Ycos,'g')

plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
