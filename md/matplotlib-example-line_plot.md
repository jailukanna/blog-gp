---
title: matplotlib example line plot (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'line plot'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import matplotlib as mpl`
* `import numpy as np`

## python line plot

Python matplotlib example: line plot

```python
import numpy as np
import matplotlib as mpl
matplotlib.use('Agg')
import matplotlib.pyplot as plt

mydata = np.loadtxt('mydata.txt')

myfig = plt.figure()
myax = myfig.add_subplot(1,1,1)

myax.plot(mydata[:,0],mydata[:,1])

myfig.savefig('mydata.png')
myfig.clf()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
