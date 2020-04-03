---
title: matplotlib example display2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'display2'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import sys`
* `import cv2`

## python display2

Python matplotlib example: display2

```python
import cv2
import sys
import numpy as np
import matplotlib.pyplot as plt

# Read the image. The first command line argument is the image
image = cv2.imread(sys.argv[1])

# Here are some alternatives if displaying with cv2 isn't working
#
#cv2.imshow("Image", image)
#
# 1) Instead of showing the image with cv2, we will save it

cv2.imwrite('display2.png', image)

# 2) Use pyplot to show the image.  Just uncomment these lines:
#
#plt.imshow(image)
#plt.show()


# Uncomment this if showing via GUI
#cv2.waitKey(0)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
