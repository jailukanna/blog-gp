---
title: PIL example get pixel (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'get pixel'

Functions in program: 
* `def get_pixel_colour(i_x, i_y):`

## python get pixel

Python PIL example: get pixel

```python
def get_pixel_colour(i_x, i_y):
	import PIL.Image # python-imaging
	import PIL.ImageStat # python-imaging
	import Xlib.display # python-xlib
	o_x_root = Xlib.display.Display().screen().root
	o_x_image = o_x_root.get_image(i_x, i_y, 1, 1, Xlib.X.ZPixmap, 0xffffffff)
	o_pil_image_rgb = PIL.Image.fromstring("RGB", (1, 1), o_x_image.data, "raw", "BGRX")
	lf_colour = PIL.ImageStat.Stat(o_pil_image_rgb).mean
	return tuple(map(int, lf_colour))
 
print(get_pixel_colour(0, 0))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
