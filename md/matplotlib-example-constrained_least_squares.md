---
title: matplotlib example constrained least squares (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'constrained least squares'


Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import pandas`
* `import seaborn as sns`
* `import matplotlib`
* `import numpy as np`

## python constrained least squares

Python matplotlib example: constrained least squares

```python
import numpy as np
from matplotlib import pyplot as plt
import matplotlib
import seaborn as sns
import pandas

font = {'family' : 'Helvetica',
        'size'   : 18}

matplotlib.rc('font', **font)

np.random.seed(222)
DPPD = 200
X = np.random.normal(0,2, (DPPD, 2))


# target w
w_target = np.random.normal(0,1, (2,1))

# constraints (overdetermined for multiple solutions)
d = np.random.normal(0,1, (1,1))
C = np.random.normal(0,1, (1,2))

# objective
y = X@w_target + np.random.normal(0, 0.2, (DPPD,1))

Y_tilde = [y, d]
X_tilde = [X, C]

# objective weights
lambd = np.asarray([1.0, 10000.0])


A_tilde = np.bmat([[np.sqrt(l)*x] for l,x in zip(lambd, X_tilde)])
y_tilde = np.bmat([[np.sqrt(l)*y] for l,y in zip(lambd, Y_tilde)])

# least squares
w_estimate = np.linalg.inv(A_tilde.T@A_tilde)@A_tilde.T@y_tilde


y_estimate = X@w_estimate

# This import registers the 3D projection, but is otherwise unused.
from mpl_toolkits.mplot3d import Axes3D  # noqa: F401 unused import

import matplotlib.pyplot as plt
import numpy as np



x1 = y_estimate.flatten()
x2 = X[:, 0].flatten()
y_estimate = y_estimate.flatten()
y_true = y.flatten()

fig = plt.figure(figsize=(20,10))
ax = fig.add_subplot(111, projection='3d')
ax.scatter(x1,x2,y_estimate, label="Estimate")
ax.scatter(x1,x2,y_true, label="Ground Truth")
ax.view_init(elev=10., azim=20)

ax.set_xlabel('X0')
ax.set_ylabel('X1')
ax.set_zlabel('Y')

plt.legend()
plt.tight_layout()
plt.savefig("3d.png")
plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
