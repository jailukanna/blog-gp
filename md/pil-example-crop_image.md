---
title: pil example crop image (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'crop image'

Functions in program: 
* `def mouse_crop(event, x, y, flags, param):`

Modules used in program: 
* `import PIL`
* `import numpy as np`
* `import cv2`

## python crop image

Python pil example: crop image

```python
'''
Installation:
Using Python 3.6
$ pip install numpy
$ pip install pillow
$ pip install opencv-python
'''

import cv2
import numpy as np
import PIL
from PIL import Image

cropping = False

x_start, y_start, x_end, y_end = 0, 0, 0, 0

basewidth = 1000
# Pass the image name/path
img = Image.open('image_name1.jpg')
wpercent = (basewidth/float(img.size[0]))
hsize = int((float(img.size[1])*float(wpercent)))
img=img.resize((basewidth, hsize), PIL.Image.ANTIALIAS)
img.save('resized_image.jpg')

image = cv2.imread('resized_image.jpg')
oriImage = image.copy()


def mouse_crop(event, x, y, flags, param):
    # grab references to the global variables
    global x_start, y_start, x_end, y_end, cropping

    # if the left mouse button was DOWN, start RECORDING
    # (x, y) coordinates and indicate that cropping is being
    if event == cv2.EVENT_LBUTTONDOWN:
        x_start, y_start, x_end, y_end = x, y, x, y
        cropping = True

    # Mouse is Moving
    elif event == cv2.EVENT_MOUSEMOVE:
        if cropping == True:
            x_end, y_end = x, y

    # if the left mouse button was released
    elif event == cv2.EVENT_LBUTTONUP:
        # record the ending (x, y) coordinates
        x_end, y_end = x, y
        cropping = False  # cropping is finished

        refPoint = [(x_start, y_start), (x_end, y_end)]

        if len(refPoint) == 2:  # when two points were found
            roi = oriImage[refPoint[0][1]:refPoint[1][1], refPoint[0][0]:refPoint[1][0]]
            cv2.imshow("Cropped", roi)
            cv2.imwrite('mousecrop.jpg', roi)


cv2.namedWindow("image")
cv2.setMouseCallback("image", mouse_crop)

while True:

    i = image.copy()

    if not cropping:
        cv2.imshow("image", image)

    elif cropping:
        cv2.rectangle(i, (x_start, y_start), (x_end, y_end), (255, 0, 0), 2)
        cv2.imshow("image", i)

    cv2.waitKey(1)

close all open windows
cv2.destroyAllWindows()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
