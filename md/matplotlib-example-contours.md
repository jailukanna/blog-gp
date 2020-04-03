---
title: matplotlib example contours (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'contours'

Functions in program: 
* `def draw_polygon(polygons_pts, h, w, title='', save=False):`
* `def draw_scatter(polygons_pts, h, w):`
* `def get_contour_pts(contours, hierarchy, verbose=0):`
* `def find_contours(img_url_local, class_id_study=0, only_classes=0,verbose=0, show_image=0):`

Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import pprint`
* `import copy`
* `import cv2`
* `import os`

## python contours

Python matplotlib example: contours

```python
import os
import cv2
import copy
import pprint
import numpy as np
from shapely.geometry import Polygon
from descartes import PolygonPatch
import matplotlib.pyplot as plt

%matplotlib inline

def find_contours(img_url_local, class_id_study=0, only_classes=0,verbose=0, show_image=0):
    if os.path.exists(img_url_local):
        img = cv2.imread(img_url_local, cv2.IMREAD_UNCHANGED)
        h, w = len(img), len(img[0])
        img_class_ids, counts = np.unique(img, return_counts=True)
        if verbose or only_classes:
            pprint.pprint(dict(zip(img_class_ids, counts)))
            if only_classes:
                return [], [], [], h, w

        for class_id in img_class_ids:
            if class_id not in [class_id_study]:
                continue
            if verbose:
                print((' - class_id : ', class_id))
            img_tmp = copy.deepcopy(img)
            img_tmp[img_tmp != class_id] = 0
            img_tmp[img_tmp == class_id] = 255
            if verbose:
                print((np.unique(img_tmp, return_counts=True)))
            if show_image:
                plt.imshow(img_tmp, cmap='gray')
                
            im2, contours, hierarchy = cv2.findContours(img_tmp, cv2.RETR_TREE,cv2.CHAIN_APPROX_SIMPLE)

            return im2, contours, hierarchy, h, w
    else:
        print((' Check image URL : ', img_url_local))
        return []

def get_contour_pts(contours, hierarchy, verbose=0):
    polygons_pts = []
    for contour_id, contour_topology in enumerate(hierarchy[0]):
        if verbose:
            print((' - hierarchy : ', contour_topology))
        next_contour    =  contour_topology[0]
        prev_contour    =  contour_topology[1]
        child_contour   =  contour_topology[2]
        parent_countour =  contour_topology[3]
        
        if child_contour == -1 and parent_countour == -1:
            polygon_pts = np.array([pts[0] for pts in contours[contour_id]])
            polygons_pts.append(polygon_pts)
        else:
            print((' - Special case noticed! Please handle.'))
    
    return polygons_pts
    
def draw_scatter(polygons_pts, h, w):
    f, axarr= plt.subplots(1, figsize=(10,10))
    axarr.set_xlim(0, w)
    axarr.set_ylim(h, 0)
    
    for polygon_pts in polygons_pts:
        plt.scatter(polygon_pts[:,0], polygon_pts[:,1]) 
    
def draw_polygon(polygons_pts, h, w, title='', save=False):
    fig, ax = plt.subplots(1, figsize=(10,10))
    ax.set_xlim(0, w)
    ax.set_ylim(h, 0)
    
    for polygon_pts in polygons_pts:
        print((polygon_pts.shape))
        if len(polygon_pts) > 2:
            poly    = Polygon(polygon_pts)
            patch   = PolygonPatch(poly, facecolor=[0,0,0.5], edgecolor=[1,1,1], alpha=0.8)
            ax.add_patch(patch)
        
    plt.title(title)
    plt.tight_layout()    


    
if __name__ == "__main__":
    ## PARAMS
    img_url_local                  = '../data/cityscapes/stuttgart_000189_000019_gtFine_labelIds.png'
    
    ## STEPS
    _, contours, hierarchy, h, w   = find_contours(img_url_local, class_id_study=4, only_classes=0
                                                   , verbose=0, show_image=0)
    if len(contours):
        polygons_pts                = get_contour_pts(contours, hierarchy, verbose=0)
        # draw_scatter(polygons_pts, h, w) 
        draw_polygon(polygons_pts, h, w)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
