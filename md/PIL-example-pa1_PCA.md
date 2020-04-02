---
title: PIL example pa1 PCA (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'pa1 PCA'

Functions in program: 
* `def PCA(data_orig, k=6):`

Modules used in program: 
* `import numpy as NP`

## python pa1 PCA

Python PIL example: pa1 PCA

```python
import numpy as NP
from scipy import linalg as LA
from matplotlib import pyplot as MPL


def PCA(data_orig, k=6):
    """
    Input: array data_orig- training set of images
           int k - representing how many principal componenets to keep [1-48]
    Output:
        np array - k first principal components in matrix form
    """
    assert k > 0
    # convert to np array
    data = NP.array(data_orig)
    size, m, n = data.shape
    # column stack images to vectors
    data = data.reshape(size, m * n)
    data = NP.float64(data)
    # subtract mean and divide by standard deviation
    data -= data.mean(axis=0)

    # data = data / (NP.std(data))  # not sure if this is needed

    sigma = NP.cov(data)  # sigma symmetric by definition
    evals, evecs = NP.linalg.eigh(sigma)  # eigh faster apparently

    idx = NP.argsort(evals[::-1])  # sort evals from big to small
    evecs = evecs[:, idx]  # sort evecs according to evals
    evals = evals[idx]
    # select only k first evecs
    evecs = evecs[:, :k]

    # print(evecs.shape,data.shape)

    # return k first principal components
    # note that there are k vectors, each (m*n)x1
    pcs = NP.dot(evecs.T, data).T
    # pcs = pcs / (NP.std(pcs))
    norm_eig = NP.linalg.norm(pcs, 2, axis=0)
    pcs = pcs / norm_eig

    return pcs.T, evecs, evals


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
