---
title: matplotlib example multiple lines plotting (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'multiple lines plotting'


Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python multiple lines plotting

Python matplotlib example: multiple lines plotting

```python
# multiple lines plotting on the same graph

import matplotlib.pyplot as plt
import numpy as np

x = np.arange(1,10)  # values from 1 to 10
y1=x+2
y2=x*6
y3=x/5
plt.plot(x,y1,x,y2,x,y3)
plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
