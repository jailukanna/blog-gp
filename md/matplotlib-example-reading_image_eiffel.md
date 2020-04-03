---
title: matplotlib example reading image eiffel (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'reading image eiffel'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import cv2 `

## python reading image eiffel

Python matplotlib example: reading image eiffel

```python
import cv2 
import matplotlib.pyplot as plt
%matplotlib inline

# read images
img1 = cv2.imread('eiffel_2.jpeg')  
img2 = cv2.imread('eiffel_1.jpg') 

img1 = cv2.cvtColor(img1, cv2.COLOR_BGR2GRAY)
img2 = cv2.cvtColor(img2, cv2.COLOR_BGR2GRAY)

figure, ax = plt.subplots(1, 2, figsize=(16, 8))

ax[0].imshow(img1, cmap='gray')
ax[1].imshow(img2, cmap='gray')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
