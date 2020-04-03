---
title: matplotlib example k-mean cluster (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'k-mean cluster'

Functions in program: 
* `def psnr(image1, image2):`

Modules used in program: 
* `import pylab`
* `import matplotlib`
* `import math`
* `import skimage`
* `import pandas as pd`
* `import numpy as np`

## python k-mean cluster

Python matplotlib example: k-mean cluster

```python
import numpy as np
import pandas as pd
from sklearn.cluster import KMeans
# %matplotlib inline
import skimage
import math

from google.colab import drive
drive.mount('/content/gdrive',force_remount = True)

from skimage.io import imread
image = imread('gdrive/My Drive/Colab Notebooks/oxford-iiit-pet/Котики-собачки/Bengal/Bengal_82.jpg')

import matplotlib
import pylab
fig = matplotlib.pyplot.gcf()
fig.set_size_inches(18.5, 10.5)
fig.savefig('test2png.png', dpi=100)
matplotlib.pylab.imshow(image)

array = np.array(image, dtype=np.float64) / 255
w, h, d = original_shape = tuple(array.shape)
matrix = pd.DataFrame(np.reshape(array, (w*h,d)),columns=['R', 'G', 'B'])

clf = KMeans(n_clusters = 7,init='k-means++',random_state=241)
clf.fit(matrix)

mean_ar = [clf.cluster_centers_[i] for i in clf.fit_predict(matrix)]
mean_image = np.reshape(mean_ar, (w,h,d))
#pylab.imshow(mean_image)

matrix['cluster'] = clf.fit_predict(matrix)
medians = matrix.groupby('cluster').median().values
median_pixels = [medians[c] for c in matrix['cluster'].values]
median_image = np.reshape(median_pixels, (w, h, d))
#matplotlib.pylab.imshow(median_image)

def psnr(image1, image2):
    mse = np.mean((image1 - image2) ** 2)
    return 10 * math.log10(1. / mse)

psnr_mean, psnr_median = psnr(array, mean_image), psnr(array, median_image)
print((psnr_mean, psnr_median))

fig = matplotlib.pyplot.gcf()
fig.set_size_inches(18.5, 10.5)
fig.savefig('test2png.png', dpi=100)
matplotlib.pylab.imshow(median_image)



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
