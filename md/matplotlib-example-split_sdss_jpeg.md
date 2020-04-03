---
title: matplotlib example split sdss jpeg (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'split sdss jpeg'

Functions in program: 
* `def main():`
* `def save_images(filename,outname):`
* `def split_image(filename):`
* `def make_color_maps():`

Modules used in program: 
* `import pylab as pl`
* `import numpy as np`
* `import matplotlib`
* `import PIL.Image`

## python split sdss jpeg

Python matplotlib example: split sdss jpeg

```python
import PIL.Image
import matplotlib
matplotlib.use("Agg")
import numpy as np
import pylab as pl

def make_color_maps():
	x=np.arange(0.,1.,0.01)
	z=np.zeros_like(x)
	R=list(zip(x,z,z))
	G=list(zip(z,x,z))
	B=list(zip(z,z,x))
	red=matplotlib.colors.ListedColormap(R,name='BlackRed')
	green=matplotlib.colors.ListedColormap(G,name='BlackGreen')
	blue=matplotlib.colors.ListedColormap(B,name='BlackBlue')
	return red,green,blue

red_colormap,green_colormap,blue_colormap = make_color_maps()

def split_image(filename):
	image=np.asarray(PIL.Image.open(filename))
	g_filter=image[:,:,0]
	r_filter=image[:,:,1]
	i_filter=image[:,:,2]
	return g_filter,r_filter,i_filter

def save_images(filename,outname):
	g,r,i=split_image(filename)
	for filter,name,cm in zip([g,r,i],['g','r','i'],[red_colormap,green_colormap,blue_colormap]):
		pl.figure(frameon=False)
		pl.imshow(filter,cmap=cm)
		ax=pl.gca()
		ax.set_axis_off()
		pl.savefig(outname+"_"+name+".png")
		pl.close()

def main():
	import sys
	input,output=sys.argv[1],sys.argv[2]
	save_images(input,output)


if __name__=="__main__":
	main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
