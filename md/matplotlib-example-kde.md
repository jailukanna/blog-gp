---
title: matplotlib example kde (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'kde'

Functions in program: 
* `def kde2(x, y, ax):`
* `def kde1(x, y, ax):`

Modules used in program: 
* `import matplotlib`
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python kde

Python matplotlib example: kde

```python
#!/usr/bin/env python3

import numpy as np
import matplotlib.pyplot as plt
import matplotlib

matplotlib.rc('legend', fontsize=8, handlelength=3)
matplotlib.rc('axes', titlesize=8)
matplotlib.rc('axes', labelsize=8)
matplotlib.rc('xtick', labelsize=8)
matplotlib.rc('ytick', labelsize=8)
matplotlib.rc('text', usetex=True)
matplotlib.rc('font', size=8, family='serif',
              style='normal', variant='normal',
              stretch='normal', weight='normal',
              serif='Times')

def kde1(x, y, ax):
    from scipy.stats import gaussian_kde
    
    # Calculate the point density
    xy = np.vstack([x,y])
    kernel = gaussian_kde(xy, bw_method='silverman')

    xmin = x.min()
    xmax = x.max()
    ymin = y.min()
    ymax = y.max()
    
    X, Y = np.mgrid[xmin:xmax:100j, ymin:ymax:100j]
    positions = np.vstack([X.ravel(), Y.ravel()])

    Z = np.reshape(kernel(positions).T, X.shape)

    ax.imshow(np.rot90(Z), cmap=plt.cm.viridis,
              extent=[xmin, xmax, ymin, ymax])
    
    ax.scatter(x, y, c='k', s=5, edgecolor='')

def kde2(x, y, ax):
    from sklearn.neighbors import KernelDensity
   
    xy = np.vstack([x,y])

    d = xy.shape[0]
    n = xy.shape[1]
    bw = (n * (d + 2) / 4.)**(-1. / (d + 4)) # silverman
    #bw = n**(-1./(d+4)) # scott
    print('bw: {}'.format(bw))
    
    kde = KernelDensity(bandwidth=bw, metric='euclidean',
                        kernel='gaussian', algorithm='ball_tree')
    kde.fit(xy.T)
    
    xmin = x.min()
    xmax = x.max()
    ymin = y.min()
    ymax = y.max()
    
    X, Y = np.mgrid[xmin:xmax:100j, ymin:ymax:100j]
    positions = np.vstack([X.ravel(), Y.ravel()])

    Z = np.reshape(np.exp(kde.score_samples(positions.T)), X.shape)

    ax.imshow(np.rot90(Z), cmap=plt.cm.viridis,
              extent=[xmin, xmax, ymin, ymax])
    
    ax.scatter(x, y, c='k', s=5, edgecolor='')

 

N1 = np.random.normal(size=500)
N2 = np.random.normal(scale=0.5, size=500)
x = N1+N2
y = N1-N2

fig, axarr = plt.subplots(1, 2)
fig.subplots_adjust(left=0.11, right=0.95, wspace=0.0, bottom=0.18)

ax = axarr[0]
kde1(x, y, ax)
ax.set_xlabel('$x$')
ax.set_ylabel('$y$')
ax.set_title('scipy')

ax.set_xlim((-2,2))
ax.set_ylim((-2,2))

ax = axarr[1]
kde2(x, y, ax)
ax.set_xlabel('$x$')
ax.set_ylabel('$y$')
ax.set_title('scikit-learn')

ax.set_xlim((-2,2))
ax.set_ylim((-2,2))

plt.tight_layout()
plt.savefig('kde.png')
plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
