---
title: matplotlib example ex (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'ex'


Modules used in program: 
* `import scipy.signal`
* `import scipy`
* `import matplotlib.pyplot`
* `import matplotlib`

## python ex

Python matplotlib example: ex

```python
import matplotlib
import matplotlib.pyplot

import scipy
import scipy.signal

from numpy import pi

[b,a] = scipy.signal.butter(2,1,analog=1)
zp = scipy.signal.freqz(b, a)

matplotlib.pyplot.plot(zp)
matplotlib.pyplot.xlim([0, pi])
matplotlib.pyplot.ylim([0, 2])
matplotlib.pyplot.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
