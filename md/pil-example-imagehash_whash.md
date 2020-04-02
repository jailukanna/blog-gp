---
title: pil example imagehash whash (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'imagehash whash'


Modules used in program: 
* `import imagehash`
* `import PIL`

## python imagehash whash

Python pil example: imagehash whash

```python
# Code for the blogpost https://fullstackml.com/2016/07/02/wavelet-image-hash-in-python/
import PIL
from PIL import Image
import imagehash

w = imagehash.whash(PIL.Image.open(‘lenna.png’))
w1 = imagehash.whash(PIL.Image.open(‘lenna1.jpg’))
w2 = imagehash.whash(PIL.Image.open(‘lenna2.jpg’))

(w — w1)/len(w.hash)**2
# > 0.03125

(w — w2)/len(w.hash)**2
# > 0.28125

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
