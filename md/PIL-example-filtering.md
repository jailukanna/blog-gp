---
title: PIL example filtering (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'filtering'


Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python filtering

Python PIL example: filtering

```python
from PIL import ImageFilter
from PIL import Image
import matplotlib.pyplot as plt
import numpy as np
from PIL import ImageMath

im = Image.open('./sample03.jpg').convert('L')

filters = [ImageFilter.EDGE_ENHANCE, ImageFilter.EDGE_ENHANCE_MORE, ImageFilter.DETAIL, ImageFilter.FIND_EDGES, ImageFilter.EMBOSS]
names = ['EDGE_ENHANCE', 'EDGE_ENHANCE_MORE', 'DETAIL', 'FIND_EDGES', 'EMBOSS']

for i in range(len(filters)):
    
#     h = im.filter(ImageFilter.SMOOTH_MORE)
    h = im.filter(filters[i])
    plt.imshow(np.array(h))
    plt.show()

    h.save(names[i] + '.jpg')

# BLUR
# CONTOUR
# DETAIL
# EDGE_ENHANCE
# EDGE_ENHANCE_MORE
# EMBOSS
# FIND_EDGES
# SMOOTH
# SMOOTH_MORE
# SHARPEN

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
