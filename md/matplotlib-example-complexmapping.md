---
title: matplotlib example complexmapping (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'complexmapping'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python complexmapping

Python matplotlib example: complexmapping

```python
import numpy as np
import matplotlib.pyplot as plt

fig = plt.figure(figsize=(11, 5))
ax1 = fig.add_subplot(1,2,1)
ax2 = fig.add_subplot(1,2,2)

#反転の円円対応
theta = np.linspace(0, 2*np.pi, 100)
r = 10
l = 3 + 8j
Z = r*(np.cos(theta) + 1j * np.sin(theta)) + l
W = 1/Z * 8 #そのまんまだとちっちゃすぎるので適当に定数倍
ax1.grid()
ax1.plot(Z.real, Z.imag, "bo", W.real, W.imag,"ro", markersize=3)

#反転の等角写像
X1,Y1 = np.meshgrid(np.array([-2,-1, 0, 1, 2]), np.linspace(-2, 2, 100))
X2,Y2 = np.meshgrid(np.linspace(-2, 2, 100), np.array([-2,-1, 0, 1, 2]))
l = 5 + 7j
Z1, Z2 = X1 + 1j * Y1 + l, X2 + 1j * Y2 + l
W1, W2 = 1/Z1, 1/Z2
ax2.grid()
ax2.plot(W1.real, W1.imag, "go", W2.real, W2.imag, "go", markersize=3)

plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
