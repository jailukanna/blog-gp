---
title: matplotlib example image draw (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'image draw'


Modules used in program: 
* `import cv2`
* `import numpy as np`

## python image draw

Python matplotlib example: image draw

```python
from matplotlib import pyplot as plt
import numpy as np
import cv2

# Load an color image in grayscale
img = cv2.imread('images/barcode_01.jpg',cv2.IMREAD_COLOR)

plt.imshow(img, cmap='gray',interpolation='bicubic')
plt.xticks([]),plt.yticks([])
plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
