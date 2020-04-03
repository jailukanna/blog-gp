---
title: matplotlib example blur2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'blur2'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import sys`
* `import cv2`

## python blur2

Python matplotlib example: blur2

```python
import cv2
import sys
import numpy as np
import matplotlib.pyplot as plt

# The first argument is the image
image = cv2.imread(sys.argv[1])

#conver to grayscale
gray_image = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

#blur it
blurred_image = cv2.GaussianBlur(image, (7,7), 0)

#
# Show all 3 images
#
# Here are some alternatives if displaying with cv2 isn't working
#
#cv2.imshow("Original Image", image)
#cv2.imshow("Gray Image", gray_image)
#cv2.imshow("Blurred Image", blurred_image)
#
# 1) Instead of showing the image with cv2, we will save it

cv2.imwrite('blur2_original.png', image)
cv2.imwrite('blur2_gray.png', gray_image)
cv2.imwrite('blur2_blurred.png', blurred_image)

# 2) Use pyplot to show the image.  Just uncomment these lines:
#
#plt.imshow(image)   # "Original Image" )
#plt.show()
#plt.imshow(gray_image)   # "Gray Image" )
#plt.show()
#plt.imshow(blurred_image)   # "Blurred Image" )
#plt.show()

# Uncomment this if showing via GUI
#cv2.waitKey(0)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
