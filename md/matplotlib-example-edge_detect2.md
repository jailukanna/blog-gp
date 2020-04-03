---
title: matplotlib example edge detect2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'edge detect2'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import sys`
* `import cv2`

## python edge detect2

Python matplotlib example: edge detect2

```python
import cv2
import sys
import numpy as np
import matplotlib.pyplot as plt

# The first argument is the image
image = cv2.imread(sys.argv[1])

#convert to grayscale
gray_image = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

#blur it
blurred_image = cv2.GaussianBlur(gray_image, (7,7), 0)

# Here are some alternatives if displaying with cv2 isn't working
#
#cv2.imshow("Original Image", image)
#
# 1) Instead of showing the image with cv2, we will save it

cv2.imwrite('edge_detect2_original.png', image)

# 2) Use pyplot to show the image.  Just uncomment these lines:
#
#plt.imshow(image)
#plt.show()

# Use low thresholds
canny = cv2.Canny(blurred_image, 10, 30)
#
#cv2.imshow("Canny with low thresholds", canny)
#
cv2.imwrite('edge_detect2_canny.png', canny)
#
# OR
#
#plt.imshow(canny)
#plt.show()

# Use high thresholds
canny2 = cv2.Canny(blurred_image, 50, 150)
#
#cv2.imshow("Canny with high thresholds", canny2)
#
cv2.imwrite('edge_detect2_canny2.png', canny2)
#
# OR
#
#plt.imshow(canny2)
#plt.show()

# Uncomment this if showing via GUI
#cv2.waitKey(0)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
