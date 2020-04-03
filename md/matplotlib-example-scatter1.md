---
title: matplotlib example scatter1 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'scatter1'


Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python scatter1

Python matplotlib example: scatter1

```python
import matplotlib.pyplot as plt
import numpy as np


nrange = np.random.standard_normal(1000)
arange = np.arange(len(nrange))
print(( nrange ))

#blank centre
plt.scatter(nrange,arange,color='1.0',edgecolors='0.0')
plt.show()
#triangular marking

plt.scatter(nrange,arange,marker='^')

plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
