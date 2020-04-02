---
title: PIL example pil imut (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'pil imut'

Functions in program: 
* `def cal_dim(pil_frame, width=None, height=None):`

Modules used in program: 
* `import cv2`
* `import imutils`

## python pil imut

Python PIL example: pil imut

```python
import imutils
from PIL import Image
import cv2


def cal_dim(pil_frame, width=None, height=None):
    # initialize the dimensions of the image to be resized and
    # grab the image size
    dim = None
    w, h = pil_frame.size

    # if both the width and height are None, then return the
    # original image

    # check to see if the width is None
    if width is None:
        # calculate the ratio of the height and construct the
        # dimensions
        r = height / float(h)
        dim = (int(w * r), height)

    # otherwise, the height is None
    else:
        # calculate the ratio of the width and construct the
        # dimensions
        r = width / float(w)
        dim = (width, int(h * r))
    return dim


if __name__ == '__main__':
    frame = cv2.imread("img.jpg")
    frame = imutils.resize(frame, width=500)    # using imutils (for cv2)

    pil_frame = Image.open("img.jpg")
    dim = cal_dim(pil_frame, width=500)          # using resize func (for PIL)

    resized = pil_frame.resize(dim)
    resized.show()

    cv2.imshow('image', frame)
    cv2.waitKey(0)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
