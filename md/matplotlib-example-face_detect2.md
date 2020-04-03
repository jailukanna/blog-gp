---
title: matplotlib example face detect2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'face detect2'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import cv2`
* `import sys`

## python face detect2

Python matplotlib example: face detect2

```python
#!/usr/bin/python

import sys
import cv2
import numpy as np
import matplotlib.pyplot as plt


imgpath = sys.argv[1]
cascasdepath = "haarcascade_frontalface_default.xml"

image = cv2.imread(imgpath)
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

face_cascade = cv2.CascadeClassifier(cascasdepath)

faces = face_cascade.detectMultiScale(
    gray,
    scaleFactor = 1.2,
    minNeighbors = 5,
    minSize = (30,30)

    )

print("The number of faces found = ", len(faces))

for (x,y,w,h) in faces:
    cv2.rectangle(image, (x,y), (x+h, y+h), (0, 255, 0), 2)

# Here are some alternatives if displaying with cv2 isn't working
#
#cv2.imshow("Faces found", image)    
#
# 1) Instead of showing the image with cv2, we will save it

cv2.imwrite('face_detect2.png', image)

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
