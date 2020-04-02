---
title: pil example resizeImage (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'resizeImage'

Functions in program: 
* `def resizeImage(srcfile, new_width=32, new_height=32):`

Modules used in program: 
* `import numpy as np`

## python resizeImage

Python pil example: resizeImage

```python
from keras.preprocessing import image
from PIL import Image, ImageOps
import numpy as np

def resizeImage(srcfile, new_width=32, new_height=32):
    '''
    Resize and crop a image to desired resolution and return the 
    data as HWC format numpy array.
    srcfile: the source image file path.
    new_width: new desired width.
    new_height: new desired height.
    '''
    pil_image = Image.open(srcfile)
    pil_image = ImageOps.fit(pil_image, (new_width, new_height), Image.ANTIALIAS)
    pil_image_rgb = pil_image.convert('RGB')
    return np.asarray(pil_image_rgb).flatten()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
