---
title: matplotlib example discreate-scatter (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'discreate-scatter'


## python discreate-scatter

Python matplotlib example: discreate-scatter

```python
# load images of the digits 0 through 5 and visualize several of them
from sklearn.datasets import load_digits
digits = load_digits(n_class=6)

# use ax.flat to draw all images in one-loop
fig, ax = plt.subplots(8, 8, figsize=(6, 6));
for i, axi in enumerate(ax.flat):
    axi.imshow(digits.images[i]);
    axi.set(xticks=[], yticks=[]);

# project the digits into 2 dimensions using IsoMap
from sklearn.manifold import Isomap
iso = Isomap(n_components=2)
projection = iso.fit_transform(digits.data)

# plot the results
# vip: use discreate cmap to distinguish different digit label's color
plt.scatter(projection[:, 0], projection[:, 1], lw=0.1,
            c=digits.target, cmap=plt.cm.get_cmap('cubehelix', 6))

# set ticks in colorbar
plt.colorbar(ticks=range(6), label='digit value')

# make ticks 1,2,3,4,5 center in the box
plt.clim(-0.5, 5.5)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
