---
title: pil example pillow mask (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'pillow mask'


## python pillow mask

Python pil example: pillow mask

```python
from PIL import Image

col = Image.open('myimage.jpg')
gry = col.convert('L')
grarray = np.asarray(gry)
bw = (grarray > grarray.mean())*255
imshow(bw)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
