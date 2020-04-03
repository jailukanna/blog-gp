---
title: matplotlib example subplot single (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'subplot single'

Functions in program: 
* `def draw_polygon(points, title='', save=False):`
* `def random():`

Modules used in program: 
* `import matplotlib.pyplot as plt`

## python subplot single

Python matplotlib example: subplot single

```python
from shapely import geometry
from descartes import PolygonPatch
import matplotlib.pyplot as plt

def random():
	f, axarr = plt.subplots(1)
	axarr.imshow(img_array)

	poly = geometry.Polygon(contour_pts)
	patch = PolygonPatch(poly, facecolor = [0,1,0], edgecolor = [0,1,0], alpha = 0.35)
	axarr.add_patch(patch)

	frame1 = plt.gca()
	frame1.axes.get_xaxis().set_visible(False)
	frame1.axes.get_yaxis().set_visible(False)
	frame1.set_xlim(0,w)
	frame1.set_ylim(0,h)
	plt.savefig(img_name, bbox_inches = 'tight', pad_inches = 0, dpi = 100)

def draw_polygon(points, title='', save=False):
    pt_max_x = np.max(points[:,0]) 
    pt_max_y = np.max(points[:,1])
    
    poly    = Polygon(pts)
    patch   = PolygonPatch(poly, facecolor=[0,0,0.5], edgecolor=[1,1,1], alpha=0.8)
    
    fig, ax = plt.subplots(1, figsize=(10,10))
    ax.set_xlim(0, pt_max_x)
    ax.set_ylim(0, pt_max_y)
    ax.add_patch(patch)
    plt.title(title)
    plt.tight_layout()
	
	if save:
		plt.savefig('img_name', bbox_inches = 'tight', pad_inches = 0, dpi = 100)

if __name__ == "__main__":
    pts = np.array([[0,0],[1,1],[0,1]]) 
    pts = np.array([[1136, 844],[1104, 913],[1550, 969], [1104, 873]]) 
    draw_polygon(pts)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
