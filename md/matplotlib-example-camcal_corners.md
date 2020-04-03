---
title: matplotlib example camcal corners (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'camcal corners'


Modules used in program: 
* `import matplotlib.image as mpimg`
* `import matplotlib.pyplot as plt`
* `import glob`
* `import cv2`
* `import numpy as np`

## python camcal corners

Python matplotlib example: camcal corners

```python
import numpy as np
import cv2
import glob
import matplotlib.pyplot as plt
import matplotlib.image as mpimg
%matplotlib inline

# prepare object points, like (0,0,0), (1,0,0), (2,0,0) ....,(6,5,0)
objp = np.zeros((6*9,3), np.float32)
objp[:,:2] = np.mgrid[0:9,0:6].T.reshape(-1,2)

# Arrays to store object points and image points from all the images.
objpoints = [] # 3d points in real world space
imgpoints = [] # 2d points in image plane.

# Make a list of calibration images
images = glob.glob('./camera_cal/calibration*.jpg')
#print(images)

# Step through the list and search for chessboard corners
for i, fname in enumerate(images, 1):
    img = cv2.imread(fname)
    gray = cv2.cvtColor(img,cv2.COLOR_BGR2GRAY)
    #print(fname)

    # Find the chessboard corners
    ret, corners = cv2.findChessboardCorners(gray, (9,6),None)

    # If found, add object points, image points
    if ret == True:
        objpoints.append(objp)
        imgpoints.append(corners)

        # Draw and save the images with corners
        img = cv2.drawChessboardCorners(img, (9,6), corners, ret)
        cv2.imwrite('./camera_cal/cali_found_corners_' + str(i) +'.jpg',img)
        cv2.waitKey(500)
        
eg_corner = mpimg.imread('./camera_cal/cali_found_corners_5.jpg')
plt.imshow(eg_corner)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
