---
title: pil example Project utils preProcess (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'Project utils preProcess'

Functions in program: 
* `def diff(img, img1):  # returns just the difference of the two images`

Modules used in program: 
* `import os`
* `import matplotlib.pyplot as plt`
* `import cv2`

## python Project utils preProcess

Python pil example: Project utils preProcess

```python
import cv2
from PIL import Image, ImageChops
import matplotlib.pyplot as plt
import os

def diff(img, img1):  # returns just the difference of the two images
    return cv2.absdiff(img, img1)


img1 = Image.open('/home/jasper/Documents/BP_Jasp/data/pg/all/T0412_S008_U014/444-1.jpg' )
img2 = Image.open('/home/jasper/Documents/BP_Jasp/data/pg/all/T0412_S008_U014/444-2300.jpg' )


img1=img1.convert('LA')
img2=img2.convert('LA')
diff = ImageChops.subtract(img1, img2)
diff = diff.point(lambda i: i * 5)
w ,h = diff.size
area = (9,70,115,229)
diff.crop(area).show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
