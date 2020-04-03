---
title: matplotlib example Plot2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'Plot2'


Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import math`

## python Plot2

Python matplotlib example: Plot2

```python
import math
import matplotlib.pyplot as plt
import numpy as np

#read from a file and plot 
X,Y =[],[]
with open( '3n+1.txt','r') as f:
    for line in f:
            x = line.split()
            X.append(x[0])
            Y.append(x[1])

plt.plot(X,Y)
plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
