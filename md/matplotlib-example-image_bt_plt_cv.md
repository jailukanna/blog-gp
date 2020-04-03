---
title: matplotlib example image bt plt cv (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'image bt plt cv'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import cv2`

## python image bt plt cv

Python matplotlib example: image bt plt cv

```python
import cv2
import matplotlib.pyplot as plt

img = cv2.imread('images/barcode_01.jpg')
b,g,r = cv2.split(img)
img2 = cv2.merge([r,g,b])
plt.subplot(121);plt.imshow(img) # expects distorted color
plt.subplot(122);plt.imshow(img2) # expect true color
plt.show()

cv2.imshow('bgr image',img) # expects true color
cv2.imshow('rgb image',img2) # expects distorted color
cv2.waitKey(0)
cv2.destroyAllWindows()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
