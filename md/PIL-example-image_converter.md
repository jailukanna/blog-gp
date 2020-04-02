---
title: PIL example image converter (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'image converter'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python image converter

Python PIL example: image converter

```python
# -*- coding: utf-8 -*-
from PIL import Image
import numpy as np
import matplotlib.pyplot as plt

# Read image to array
image = np.array(Image.open('images/image.jpg').convert('L'))

# Create a new figure
plt.figure()

# Convert to gray scale
plt.gray()

# Take contours with origin upper left corner
plt.contour(image, origin='image')
plt.axis('equal')
plt.axis('off')

# Show image
plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
