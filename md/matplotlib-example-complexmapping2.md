---
title: matplotlib example complexmapping2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'complexmapping2'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python complexmapping2

Python matplotlib example: complexmapping2

```python
import numpy as np
import matplotlib.pyplot as plt

#f(Z)=1/Z 複素平面の写像
X,Y = np.meshgrid(np.linspace(-20, 20, 1000), np.linspace(-20, 20, 1000))
Z = X + 1j * Y
W = 1/Z #Z**2 Z**3
plt.figure(figsize=(5, 5))
plt.grid()
plt.xlim(-4, 4)
plt.ylim(-4, 4)
plt.plot(W.real, W.imag, "go",  markersize=3)
plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
