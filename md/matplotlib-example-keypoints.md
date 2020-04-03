---
title: matplotlib example keypoints (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'keypoints'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import cv2 `

## python keypoints

Python matplotlib example: keypoints

```python
import cv2 
import matplotlib.pyplot as plt
%matplotlib inline

#reading image
img1 = cv2.imread('eiffel_2.jpeg')  
gray1 = cv2.cvtColor(img1, cv2.COLOR_BGR2GRAY)

#keypoints
sift = cv2.xfeatures2d.SIFT_create()
keypoints_1, descriptors_1 = sift.detectAndCompute(img1,None)

img_1 = cv2.drawKeypoints(gray1,keypoints_1,img1)
plt.imshow(img_1)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
