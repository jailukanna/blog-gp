---
title: matplotlib example mpl latex toy (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'mpl latex toy'


Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import matplotlib`

## python mpl latex toy

Python matplotlib example: mpl latex toy

```python
import matplotlib
import matplotlib.pyplot as plt
#nb requires latex installed on the machine, see http://matplotlib.org/users/usetex.html
from matplotlib import rc
import numpy as np

rc('text', usetex=True)
matplotlib.rcParams['mathtext.fontset'] = 'custom'
matplotlib.rcParams['mathtext.rm'] = 'Bitstream Vera Sans'
matplotlib.rcParams['mathtext.it'] = 'Bitstream Vera Sans:italic'
matplotlib.rcParams['mathtext.bf'] = 'Bitstream Vera Sans:bold'
plt.close('all')
fig=plt.figure()
ax=fig.add_subplot(1, 1,1)
cs1=ax.contourf(np.random.rand(10,10))
cb1=plt.colorbar(cs1,orientation='horizontal')
cb1.set_label(r"$\displaystyle ^{\circ} C$")
plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
