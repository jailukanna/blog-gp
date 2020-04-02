---
title: pil example png cropper (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'png cropper'

Functions in program: 
* `def crop(png_image_name):`

Modules used in program: 
* `import numpy as np`

## python png cropper

Python pil example: png cropper

```python
from PIL import Image
import numpy as np
from os import listdir


def crop(png_image_name):
    pil_image = Image.open(png_image_name)
    np_array = np.array(pil_image)
    blank_element = [255, 255, 255, 0]
    mask = np_array != blank_element
    coords = np.argwhere(mask)
    x0, y0, z0 = coords.min(axis=0)
    x1, y1, z1 = coords.max(axis=0) + 1
    cropped_box = np_array[x0:x1, y0:y1, z0:z1]
    pil_image = Image.fromarray(cropped_box, 'RGBA')
    pil_image.save(png_image_name)


for f in listdir('.'):
    if f.endswith('.png'):
        crop(f)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
