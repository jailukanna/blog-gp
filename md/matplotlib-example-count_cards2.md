---
title: matplotlib example count cards2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'count cards2'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import sys`
* `import cv2`

## python count cards2

Python matplotlib example: count cards2

```python
import cv2
import sys
import numpy as np
import matplotlib.pyplot as plt

# Read the image
image = cv2.imread("cards.jpg")

#convert to grayscale
gray_image = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

#blur it
blurred_image = cv2.GaussianBlur(gray_image, (7,7), 0)

# Show both our images
#
# Here are some alternatives if displaying with cv2 isn't working
#
#cv2.imshow("Original image", image)
#cv2.imshow("Blurred image", blurred_image)
#
# 1) Instead of showing the image with cv2, we will save it

cv2.imwrite('count_cards2_original.png', image)
cv2.imwrite('count_cards2_blurred.png', blurred_image)

# 2) Use pyplot to show the image.  Just uncomment these lines:
#
#plt.imshow(image)
#plt.show()
#plt.imshow(blurred_image)
#plt.show()

# Run the Canny edge detector
canny = cv2.Canny(blurred_image, 30, 100)

#
#cv2.imshow("Canny", canny)
#
cv2.imwrite('count_cards2_canny.png', canny)
#
# OR
#
#plt.imshow(canny)
#plt.show()

im, contours, hierarchy= cv2.findContours(canny, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)

print("Number of objects found = ", len(contours))

cv2.drawContours(image, contours, -1, (0,255,0), 2)
#
#cv2.imshow("objects Found", image)
#
cv2.imwrite('count_cards2_objects.png', image)
#
# OR
#
#plt.imshow(image)
#plt.show()

# Uncomment this if showing via GUI
#cv2.waitKey(0)
#plt.clf()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
