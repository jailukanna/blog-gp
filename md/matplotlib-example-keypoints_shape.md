---
title: matplotlib example keypoints shape (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'keypoints shape'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import cv2 `

## python keypoints shape

Python matplotlib example: keypoints shape

```python
import cv2 
import matplotlib.pyplot as plt
%matplotlib inline

# read images
img1 = cv2.imread('eiffel_2.jpeg')  
img2 = cv2.imread('eiffel_1.jpg') 

img1 = cv2.cvtColor(img1, cv2.COLOR_BGR2GRAY)
img2 = cv2.cvtColor(img2, cv2.COLOR_BGR2GRAY)

#sift
sift = cv2.xfeatures2d.SIFT_create()

keypoints_1, descriptors_1 = sift.detectAndCompute(img1,None)
keypoints_2, descriptors_2 = sift.detectAndCompute(img2,None)

len(keypoints_1), len(keypoints_2)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
