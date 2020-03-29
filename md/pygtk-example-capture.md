---
title: pygtk example capture (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'capture'


Modules used in program: 
* `import sys`
* `import gobject`
* `import threading`
* `import time`
* `import gtk`
* `import pygtk`

## python capture

Python pygtk example: capture

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import pygtk
import gtk
import time
import threading
import gobject
import sys

class graphicsLCD:
	def __init__(self, scale=1):
		self.scale = scale
		self.window = gtk.Window(gtk.WINDOW_TOPLEVEL)
		self.window.set_size_request(128*self.scale, 0)
		self.window.show()
		self.window.connect("event", self.get_coordinates)
		self.x = 0
		self.y = 0
		self.disp_array = [[0 for j in range(128)] for i in range(64)];
		self.temp_array = [[0 for j in range(128)] for i in range(64)];
		gobject.timeout_add(1, self.timeout)

	def main(self):
		gtk.main()

	def get_coordinates(self, widget, event) :
		" ウインドウ座標取得 "
		(self.x, self.y) = self.window.get_position()

	def timeout(self):
		" キャプチャ "
		root = gtk.gdk.get_default_root_window()
		pixbuf = gtk.gdk.Pixbuf(gtk.gdk.COLORSPACE_RGB, False, 8, 128*self.scale, 64*self.scale)
		pixbuf.get_from_drawable(root, gtk.gdk.colormap_get_system(),
			self.x, self.y+30, 0, 0, 128*self.scale, 64*self.scale)
		pixels = pixbuf.get_pixels()

		" グレースケール化 "
		for i in range(64):
			for j in range(128):
				self.temp_array[i][j] =(
					(ord(pixels[i*self.scale*128*self.scale*3+j*self.scale*3])*0.298912
					+ord(pixels[i*self.scale*128*self.scale*3+j*self.scale*3+1])*0.586611
					+ord(pixels[i*self.scale*128*self.scale*3+j*self.scale*3+2])*0.114478))

		" しきい値（平均を求める） "
		sum = 0
		for i in range(64):
			for j in range(128):
				sum += self.temp_array[i][j]
		siki = sum/(64*128)

		" 二値化 "
		array = [[0 for j in range(128)] for i in range(64)];
		for i in range(64):
			for j in range(128):
				if self.temp_array[i][j] > siki :
					array[i][j] = 0
				else :
					array[i][j] = 1

		for i in range(64):
			for j in range(128):
				if array[i][j] == 0:
					sys.stdout.write('  ')
				else :
					sys.stdout.write('**')
			print
				
		self.disp_array = array

		gobject.timeout_add(100, self.timeout)

print(__name__)
if __name__ == "__main__":
	glcd = graphicsLCD(4)
	glcd.main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
