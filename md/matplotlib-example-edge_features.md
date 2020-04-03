---
title: matplotlib example edge features (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'edge features'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python edge features

Python matplotlib example: edge features

```python
#importing the required libraries
import numpy as np
from skimage.io import imread, imshow
from skimage.filters import prewitt_h,prewitt_v
import matplotlib.pyplot as plt
%matplotlib inline

#reading the image 
image = imread('puppy.jpeg',as_gray=True)

#calculating horizontal edges using prewitt kernel
edges_prewitt_horizontal = prewitt_h(image)
#calculating vertical edges using prewitt kernel
edges_prewitt_vertical = prewitt_v(image)

imshow(edges_prewitt_vertical, cmap='gray')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
