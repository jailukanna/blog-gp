---
title: matplotlib example matplotlib tutorial 2.%2520Brainwave%2520Plot (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'matplotlib tutorial 2.%2520Brainwave%2520Plot'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python matplotlib tutorial 2.%2520Brainwave%2520Plot

Python matplotlib example: matplotlib tutorial 2.%2520Brainwave%2520Plot

```python

import numpy as np
import matplotlib.pyplot as plt

filename = "sampleEEG.txt"  ## 10sec brainwave from Discovery24e

f_io = open(filename, 'r+')

header = f_io.readline().strip('\n').split(',')

N = 256     # sampling rate
iter = 10   # iteration times

for i in xrange(iter):              ## iterate plot
    count = 0
    array = np.zeros((N,len(header)))

    while count < N:
        array[count,:] = f_io.readline().strip('\n').split(',')
        count +=1

    plt.plot(array[:,0],array[:,2], 'r-')
    plt.show()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
