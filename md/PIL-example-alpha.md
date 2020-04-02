---
title: PIL example alpha (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'alpha'


Modules used in program: 
* `import torchvision as tv`
* `import numpy as np`
* `import pydicom`
* `import matplotlib as mpl`
* `import matplotlib.pyplot as plt`

## python alpha

Python PIL example: alpha

```python
import matplotlib.pyplot as plt
import matplotlib as mpl
from matplotlib.patches import Rectangle
import pydicom
import numpy as np
import torchvision as tv

img = pydicom.dcmread('/Users/nikhil/workspace/src/rsna/dcm/00aecb01-a116-45a2-956c-08d2fa55433f.dcm').pixel_array
img = np.expand_dims(img, -1)
img_pil = tv.transforms.functional.to_pil_image(img)

box = 288.0, 322., 94., 135.0
x, y, w, h = box
angle = 0
patch = Rectangle((x,y), w, h, color='red', ls='dashed', angle=angle, fill=False, lw=4, joinstyle='round', alpha=0.6)

target = np.zeros((1024, 1024))
xi,yi,hi,wi = int(x), int(y), int(h), int(w)
for r in range(xi, xi+wi+1):
   for c in range(yi, yi+hi+1):
       target[r,c]=255
target = np.expand_dims(target, -1)
target = target.astype('uint8')
target = tv.transforms.functional.to_pil_image(target)

fig, ax = plt.subplots()
ax.plot(target)
ax.imshow(target)
ax.add_patch(patch)
plt.show()

fig, ax = plt.subplots()
ax.imshow(img_pil)
ax.imshow(target, alpha=0.5)
plt.show()

fig, ax = plt.subplots()
ax.imshow(img_pil)
ax.imshow(target, alpha=0.1)
plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
