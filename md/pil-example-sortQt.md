---
title: pil example sortQt (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'sortQt'

Functions in program: 
* `def select_image():`
* `def prevPic(event=None):`
* `def nextPic(event=None):`
* `def cpToLabel(event=None):`
* `def refresh():`

Modules used in program: 
* `import os`
* `import glob`
* `import cv2`
* `import tkinter.filedialog as tkFileDialog`

## python sortQt

Python pil example: sortQt

```python

# import the necessary packages
from tkinter import *
from PIL import Image
from PIL import ImageTk
import tkinter.filedialog as tkFileDialog
import cv2
import glob
from shutil import copyfile
import os

def refresh():
	global panelA
	global image_list, image_index
	global lbl1
	global strIndex

	image = cv2.imread(image_list[image_index])
	#image = cv2.resize(image) # JFS
	image = cv2.resize(image, (0, 0), fx=0.35, fy=0.35) # JFS
	image = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)

	# convert the images to PIL format...
	image = Image.fromarray(image)

	# ...and then to ImageTk format
	image = ImageTk.PhotoImage(image)

	strIndex.set("{0}".format(image_index))
	#lbl1.text = image_index
	# if the panels are None, initialize them
	if panelA is None:
		# the first panel will store our original image
		panelA = Label(image=image)
		panelA.image = image
		#panelA.pack(side="left", padx=10, pady=10)
		panelA.grid(column=1, rowspan=3)
	else:
		panelA.configure(image=image)
		panelA.image = image
	return

def cpToLabel(event=None):
	global image_list, image_index

	copyfile(image_list[image_index], "/media/jfsix/E22A67922A676311/toLabel/"+os.path.basename(image_list[image_index]))
	refresh()

	return

def nextPic(event=None):
	global image_list, image_index


	image_index += 1
	image_index = image_index % len(image_list)

	refresh()

	return

def prevPic(event=None):
	global image_list, image_index


	image_index -= 1
	if image_index <0:
		image_index = len(image_list)
	else:
		image_index = image_index % len(image_list)

	refresh()

	return


def select_image():
	# grab a reference to the image panels
	#global panelA#, panelB
	global image_list, image_index

	# open a file chooser dialog and allow the user to select an input
	# image
	#path = tkFileDialog.askopenfilename()
	path = tkFileDialog.askdirectory(initialdir="/media/jfsix/E22A67922A676311/GOPRO")
	if len(path)>0:
		image_list= glob.glob(path + "/**/*.JPG", recursive=True)
		image_index = 0
		

	# ensure a file path was selected
	if len(path) < 0:
		# load the image from disk, convert it to grayscale, and detect
		# edges in it
		image = cv2.imread(path)
		image = cv2.resize(image, (0, 0), fx=0.25, fy=0.25) # JFS
		gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
		edged = cv2.Canny(gray, 50, 100)

		# OpenCV represents images in BGR order; however PIL represents
		# images in RGB order, so we need to swap the channels
		image = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)

		# convert the images to PIL format...
		image = Image.fromarray(image)
		edged = Image.fromarray(edged)

		# ...and then to ImageTk format
		image = ImageTk.PhotoImage(image)
		edged = ImageTk.PhotoImage(edged)

		# if the panels are None, initialize them
		if panelA is None or panelB is None:
			# the first panel will store our original image
			panelA = Label(image=image)
			panelA.image = image
			panelA.pack(side="left", padx=10, pady=10)

			# while the second panel will store the edge map
			panelB = Label(image=edged)
			panelB.image = edged
			panelB.pack(side="right", padx=10, pady=10)

		# otherwise, update the image panels
		else:
			# update the pannels
			panelA.configure(image=image)
			panelB.configure(image=edged)
			panelA.image = image
			panelB.image = edged

# initialize the window toolkit along with the two image panels
root = Tk()
panelA = None
panelB = None
image_list = []
image_index = 0
strIndex = StringVar()
strIndex.set(image_index)


buttonframe = Frame(root)
buttonframe.grid(row=5, column=2)        

panelA = Label(buttonframe)
panelA.grid(column=1, rowspan=5)

lbl1 = Label(buttonframe, textvariable=strIndex).grid(row=3, column=0)
btn1 = Button(buttonframe, text="Next", command=nextPic).grid(row=0, column=0)
btn2 = Button(buttonframe, text="Previous", command=prevPic).grid(row=1, column=0)
btn = Button(buttonframe, text="Select an path", command=select_image).grid(row=2, column=0)
btn3 = Button(buttonframe, text="ToLabel...", command=cpToLabel).grid(row=4, column=0)

root.bind('n', nextPic)
root.bind('p', prevPic)
root.bind('a', cpToLabel)
root.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
