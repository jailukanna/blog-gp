---
title: matplotlib example reading image (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'reading image'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import pandas as pd`

## python reading image

Python matplotlib example: reading image

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline
from skimage.io import imread, imshow

image = imread('image_8_original.png', as_gray=True)
imshow(image)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
