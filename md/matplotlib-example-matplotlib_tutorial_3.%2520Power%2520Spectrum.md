---
title: matplotlib example matplotlib tutorial 3.%2520Power%2520Spectrum (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'matplotlib tutorial 3.%2520Power%2520Spectrum'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python matplotlib tutorial 3.%2520Power%2520Spectrum

Python matplotlib example: matplotlib tutorial 3.%2520Power%2520Spectrum

```python

import numpy as np
import matplotlib.pyplot as plt
from numpy.fft import fft

filename = "sampleEEG.txt"  ## 10sec brainwave from Discovery24e

f_io = open(filename, 'r+')

header = f_io.readline().strip('\n').split(',')

fs = 256     # sampling rate
iter = 10   # iteration times

for i in xrange(iter):              ## iterate plot
    count = 0
    array = np.zeros((fs,len(header)))

    while count < fs:
        array[count,:] = f_io.readline().strip('\n').split(',')
        count +=1

    eeg = array[:,2]
    L = len(eeg)
    Y = fft(eeg)
    f = fs * np.linspace(1,L/5,L/5)/L

    plt.plot(f, abs(Y[0: L/5]))
    plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
