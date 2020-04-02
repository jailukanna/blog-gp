---
title: PIL example pil opencv (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'pil opencv'

Functions in program: 
* `def boxes_on_faces(cv_image, faces):`
* `def str2pil(image_data):`
* `def pil2cv(pil_image):`

Modules used in program: 
* `import cStringIO as StringIO`
* `import Image`
* `import cv`

## python pil opencv

Python PIL example: pil opencv

```python
###################################################################################################
import cv
import Image
import cStringIO as StringIO

def pil2cv(pil_image):
    channels = 1 if pil_image.mode == 'L' else 3
    cv_im = cv.CreateImageHeader(pil_image.size, cv.IPL_DEPTH_8U, channels)
    cv.SetData(cv_im, pil_image.tostring())
    return cv_im

def str2pil(image_data):
    return Image.open(StringIO.StringIO(image_data))

def boxes_on_faces(cv_image, faces):
    for (x, y, w, h), n in faces:
        pt1 = (int(x), int(y))
        pt2 = (int((x + w)), int((y + h)))
        cv.Rectangle(cv_image, pt1, pt2, cv.RGB(255, 0, 0), 3, 8, 0)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
