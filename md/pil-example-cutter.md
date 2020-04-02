---
title: pil example cutter (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'cutter'

Functions in program: 
* `def main(filename = None):`

## python cutter

Python pil example: cutter

```python
#!/usr/bin/env python3
from re import compile as re_compile
from select import select
from sys import stdin
from time import time
from os.path import exists
from os import listdir, makedirs
from PIL.Image import frombytes, open as fromfile, eval as image_eval, merge as image_merge
from PIL.ImageTk import PhotoImage
from PIL.ImageOps import invert, autocontrast, grayscale, equalize, solarize
from tkinter import Frame, Button, Tk, Label, Canvas, BOTH, TOP, Checkbutton, OptionMenu, StringVar, BooleanVar, Menu, IntVar, LEFT, RIGHT, TOP

''' Program to edit two squares in an image for a two-page book
'''

OVERLAP = 10

class Cap(Frame):
	def __init__(self, filename):
		' set defaults, create widgets, bind callbacks, start live view '
		self.root = Tk()
		# menu:
		self.menu = Menu(self.root)
		self.root.config(menu=self.menu)
		# bind global keypresses:
		self.root.bind('q', lambda e: self.root.quit())
		self.root.bind('x', lambda e: self.root.quit())
		#self.root.bind('<Enter>', lambda e: self.root.quit())
		self.root.bind('<Left>', lambda e: self.move(-1, 0))
		self.root.bind('<Right>', lambda e: self.move(1, 0))
		self.root.bind('<Up>', lambda e: self.move(0, -1))
		self.root.bind('<Down>', lambda e: self.move(0, 1))
		self.root.bind('<Prior>', lambda e: self.move(0, 0, 1))
		self.root.bind('<Next>', lambda e: self.move(0, 0, -1))
		#
		Frame.__init__(self, self.root)
		self.grid()
		self.x_canvas = Canvas(self, )
		self.x_canvas.pack(side=TOP)
		self.x_canvas.bind('<Button-1>', lambda e: self.move(0, 0))
		self.ox = self.oy = 0
		self.image = fromfile(filename)
		self.orig_dx, self.orig_dy = self.image.size
		self.image.thumbnail((800, 800, ), )
		self.photo = PhotoImage(self.image)
		self.dx, self.dy = self.image.size
		#self.scale = self.dx / self.orig_dx
		self.scale = self.dy / self.orig_dy
		self.x_canvas.create_image(self.dx/2, self.dy/2, image=self.photo)
		# we look for two squares fitting into the rectangle
		if self.dx / 2 + (OVERLAP/2) < self.dy:
			self.ds = self.dx / 2 + (OVERLAP/2) # delta square
			self.oy = (self.dy - self.ds) / 2
		else:
			self.ds = self.dy
			self.ox = (self.dx - self.ds) / 2
		l, r = self.calc()
		self.left_page = self.x_canvas.create_rectangle(*l, outline="blue", width=1)
		self.right_page = self.x_canvas.create_rectangle(*r, outline="red", width=1)
		self.x_canvas.config(width=self.dx, height=self.dy, )
		self.x_canvas.pack(side=TOP)

	def move(self, x, y, d=0):
		self.ox += x
		self.oy += y
		self.ds += d
		l, r = self.calc()
		self.x_canvas.coords(self.left_page, l)
		self.x_canvas.coords(self.right_page, r)

	def calc(self):
		oxL = self.ox
		oy = self.oy
		oxR = self.ox + self.ds - OVERLAP
		return (
			(oxL, oy, oxL + self.ds, oy + self.ds, ),
			(oxR, oy, oxR + self.ds, oy + self.ds, ),
			)

	def calc_real(self):
		oxL = int(self.ox / self.scale)
		oy = int(self.oy / self.scale)
		oxR = int((self.ox + self.ds - OVERLAP) / self.scale)
		ds = int(self.ds / self.scale)
		return (
			(oxL, self.orig_dy - (oy + ds), self.orig_dx - (oxL + ds), oy, ),
			(oxR, self.orig_dy - (oy + ds), self.orig_dx - (oxR + ds), oy, ),
			)

def main(filename = None):
	' main start point of the program '
	if not filename:
		pattern = re_compile(r'\\AddToShipoutPictureBG\*{\\includegraphics\[[^\]]*\]{(?P<filename>[^}]*)}}')
		# try to guess from stdin / latex pattern
		for x in stdin.readlines():
			match = pattern.match(x)
			if match:
				filename = match.groupdict()['filename']
				break
	app = Cap(filename)
	app.mainloop()
	app.root.destroy()
	left, right = app.calc_real()
	left, right = list(left), list(right),
	print('\\AddToShipoutPictureBG*{{\\includegraphics[width=\\paperwidth,height=\\paperheight,keepaspectratio,clip,trim={}pt {}pt {}pt {}pt]{{{}}}}}'.format(*left + [filename, ]))
	print('\\null\\newpage')
	print('\\AddToShipoutPictureBG*{{\\includegraphics[width=\\paperwidth,height=\\paperheight,keepaspectratio,clip,trim={}pt {}pt {}pt {}pt]{{{}}}}}'.format(*right + [filename, ]))
	print('\\null\\newpage')

if __name__ == '__main__':
	from sys import argv
	main(*argv[1:])
# vim:tw=0:nowrap


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
