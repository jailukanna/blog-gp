---
title: pil example landmark tps (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'landmark tps'

Functions in program: 
* `def morphops_tps(landmark_from, landmark_to, image, to_size):`
* `def tps_warp_np(image, c_src, c_dst, dest_shape, bg=(255,255,255,0), reduced=True):`
* `def tps_warp_pil(image : Image, c_src, c_dst, dest_size, bg=(255,255,255,0), reduced=True):`

Modules used in program: 
* `import cv2`
* `import thinplate as tps`
* `import cv2`
* `import morphops`
* `import thinplate as tps`

## python landmark tps

Python pil example: landmark tps

```python
import thinplate as tps
from PIL import Image
import morphops
import cv2
# helper function for https://github.com/cheind/py-thin-plate-spline

import thinplate as tps
from PIL import Image
import cv2

def tps_warp_pil(image : Image, c_src, c_dst, dest_size, bg=(255,255,255,0), reduced=True):
    '''
    helper function for https://github.com/cheind/py-thin-plate-spline
    Returns TPS warped image with PIL image as input, 
    PIL is w x h while numpy is h x w. This function keeps the sanity between diamensions

            Parameters:
                    image (Image): PIL image
                    c_src (ndarray): centroid point of the source image (x, y). Normalized 0 to 1
                    c_dst (ndarray): centroid point of the desired destination image (x, y). Normalized 0 to 1
                    dest_size (tuple): Optional, destination image size, (w x h). if not provided source image size is taken.
                    bg (tuple or cv2 color, cv2 bordertype): bacground color to be filled 
                    reduced (bool) : Refer to tps.tps_theta_from_points for reference

            Returns:
                    warped_output (Image): Warped PIL image 
    '''
    dest_shape = dest_size[::-1] or image.size[::-1]
    theta = tps.tps_theta_from_points(c_src, c_dst, reduced=reduced)
    grid = tps.tps_grid(theta, c_dst, dest_shape)
    mapx, mapy = tps.tps_grid_to_remap(grid, image.size[::-1])
    warped_output = cv2.remap(np.array(image), mapx, mapy, cv2.INTER_CUBIC, borderValue=bg)
    return Image.fromarray(warped_output)



def tps_warp_np(image, c_src, c_dst, dest_shape, bg=(255,255,255,0), reduced=True):
    '''
    helper function for https://github.com/cheind/py-thin-plate-spline
    Returns TPS warped image with numpy image as input, 
    numpy image shapes are h x w. This function keeps the sanity between diamensions

            Parameters:
                    image (ndarray): numpy array image
                    c_src (ndarray): centroid point of the source image (x, y). Normalized 0 to 1
                    c_dst (ndarray): centroid point of the desired destination image (x, y). Normalized 0 to 1
                    dest_shape (tuple): Optional, destination image shape in numpy diamension sequence, (h, w). if not provided source image size is taken.
                    bg (tuple or cv2 color, cv2 bordertype): bacground color to be filled 
                    reduced (bool) : Refer to tps.tps_theta_from_points for reference

            Returns:
                    warped_output (ndarray): warped numpy image
    '''
    dest_shape = dest_shape or image.shape[:2]
    theta = tps.tps_theta_from_points(c_src, c_dst, reduced=reduced)
    grid = tps.tps_grid(theta, c_dst, dest_shape)
    mapx, mapy = tps.tps_grid_to_remap(grid, image.shape[:2])
    return cv2.remap(image, mapx, mapy, cv2.INTER_CUBIC, borderValue=bg)



def morphops_tps(landmark_from, landmark_to, image, to_size):
    '''
    morphops tps, considerably slower than https://github.com/cheind/py-thin-plate-spline
    '''
    xv, yv = np.meshgrid(np.arange(to_size[0]), np.arange(to_size[1]))

    pts = np.stack([xv.flatten(), yv.flatten()]).T
    
    maps = morphops.tps_warp(landmark_to, landmark_from, pts)

    maps2 = maps.reshape((to_size[1], to_size[0] ,2))
    mapx = maps2[...,1].astype(np.float32)
    mapy = maps2[...,0].astype(np.float32)
    
    img = cv2.remap(np.array(image), mapy, mapx, cv2.INTER_CUBIC, borderValue=(255,255,255,0))

    warped = Image.fromarray(img)
    return warped

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
