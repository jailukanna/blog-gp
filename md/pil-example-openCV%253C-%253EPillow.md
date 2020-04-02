---
title: pil example openCV%253C-%253EPillow (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'openCV%253C-%253EPillow'


Modules used in program: 
* `import numpy  `
* `import cv2  `
* `import numpy  `
* `import cv2  `

## python openCV%253C-%253EPillow

Python pil example: openCV%253C-%253EPillow

```python
# OpenCV轉換成PIL.Image格式：

import cv2  
from PIL import Image  
import numpy  
  
img = cv2.imread("plane.jpg")  
cv2.imshow("OpenCV",img)  
image = Image.fromarray(cv2.cvtColor(img,cv2.COLOR_BGR2RGB))  
image.show()  
cv2.waitKey()  


# PIL.Image轉換成OpenCV格式：

import cv2  
from PIL import Image  
import numpy  
  
image = Image.open("plane.jpg")  
image.show()  
img = cv2.cvtColor(numpy.asarray(image),cv2.COLOR_RGB2BGR)  
cv2.imshow("OpenCV",img)  
cv2.waitKey()  

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
