---
title: PIL example photoeditor (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'photoeditor'


Modules used in program: 
* `import os`

## python photoeditor

Python PIL example: photoeditor

```python
#!/Users/nathan/Developer/venv/bin/python
import os
from swampy.Gui import *
from Tkinter import PhotoImage
from PIL import Image as PIL
from PIL import ImageTk
from PIL import ImageEnhance

class Draggable(Item):
    """A Canvas Item with bindings for dragging and dropping.

    Given an item, Draggable(item) creates bindings and returns
    a Draggable object with the same canvas and tag as the original.
    """
    def __init__(self, item):
        self.canvas = item.canvas
        self.tag = item.tag
        self.bind('<ButtonPress-1>', self.select)
        self.bind('<B1-Motion>', self.drag)
        self.bind('<ButtonRelease-1>', self.drop)
        self.test=False
        if self.test:
        	self.test.delete()
        self.boxy()
		
    # the following event handlers take an event object as a parameter
    def boxy(self):
    	x=self.bbox()
        self.test=self.canvas.rectangle(x)
        self.test.config(outline='green', width=5)
        
    def select(self, event):
        """Selects this item for dragging."""
        self.dragx = event.x
        self.dragy = event.y
        if self.test:
        	self.test.delete()
        
        
        
    def drag(self, event):
        """Move this item using the pixel coordinates in the event object."""
        # see how far we have moved
        dx = event.x - self.dragx
        dy = event.y - self.dragy

        # save the current drag coordinates
        self.dragx = event.x
        self.dragy = event.y

        # move the item 
        self.move(dx, dy)

    def drop(self, event):
        """Drops this item."""
        self.boxy()


class Imgr(Gui):
	def __init__(self, dirname='/Users/nathan/Desktop/a'):
		Gui.__init__(self)
		self.row()
		self.canvas=self.ca(width=400, height=400)
		self.col()
		self.gr(cols=2)
		self.tester=self.bu(text='next', command=self.increment)
		self.counter=0
		self.prev=self.bu(text='prev', command=self.decrement)
		self.keep_going = True
		self.forward=True
		self.drag=self.bu(text='drag', command=self.make_drag)
		self.exit=self.bu(text='quit', command=self.leave)
		self.dirname=dirname
		self.get_image_files()
		self.endgr()
		self.row([0,1], pady=30)
		self.resize=self.bu(text='resize', command=self.change_size)
		self.entry_size=self.en()
		self.endrow()
		self.bwb=self.bu(text='make B&W', command=self.black_white)

	def leave(self):
		self.keep_going = False
		self.destroy()
		
	def decrement(self):
		self.forward=False
		self.counter-=1
		self.canvas.clear()
		self.quit()
		
	def increment(self):
		self.forward=True
		self.counter+=1
		self.canvas.clear()
		self.quit()
		
	def directional(self):
		if self.forward:
			self.increment()
		if not self.forward:
			self.decrement()
		
		
	def get_image_files(self):
		self.files=os.listdir(self.dirname)
		
	def get_images(self):
		while self.keep_going:
			file=self.files[self.counter]
			self.f=self.dirname+'/'+str(file)
			self.now_new=True
			try:
				self.display()
				self.mainloop()
			except:
				self.directional()
			if self.counter>=len(self.files):
				self.leave()
	
	def display(self):
		if self.drag.cget('text')=='undrag':
			self.drag.config(text='drag')
		self.opener()
		self.resizer()
		self.converter()
		self.main=self.canvas.image([0,0],image=self.timage)
		
		
	def opener(self):
		self.file=PIL.open(self.f)
		self.bwb.config(text='make B&W image')
	
	def openbw(self):
		self.file=PIL.open(self.f).convert("L")
		self.bwb.config(text='make color image')
		
	def converter(self):
		self.timage=ImageTk.PhotoImage(self.new)
	
	
	def change_size(self):
		self.now_new=False
		x=int(self.entry_size.get())/100.00
		self.width=int(self.width*x)
		self.height=int(self.height*x)
		self.new=self.file.resize((self.width, self.height))
		self.converter()
		x=self.main.coords()[0]
		self.canvas.clear()
		self.main.delete()
		self.main=self.canvas.image(x,image=self.timage)
		if self.drag.cget('text')=='undrag':
			self.main=Draggable(self.main)
		
	
	def resizer(self):
		size = self.file.size
		self.width=size[0]
		self.height=size[1]
		while self.width>400 or self.height>400:
			self.width=int(self.width*.80)
			self.height=int(self.height*.80)
		self.new=self.file.resize((self.width, self.height))
		
	def black_white(self):
		current=self.file.mode
		if self.now_new:
			if current != "L":
				self.openbw()
				self.resizer()
				self.converter()
				x=self.main.coords()[0]
				self.canvas.clear()
				self.main.delete()
				self.main=self.canvas.image(x,image=self.timage)
			else:
				self.opener()
				self.resizer()
				self.converter()
				x=self.main.coords()[0]
				self.canvas.clear()
				self.main.delete()
				self.main=self.canvas.image(x,image=self.timage)
		else:
			if current != "L":
				self.openbw()
				self.new=self.file.resize((self.width, self.height))
				self.converter()
				x=self.main.coords()[0]
				self.canvas.clear()
				self.main.delete()
				self.main=self.canvas.image(x,image=self.timage)
			else:
				self.opener()
				self.new=self.file.resize((self.width, self.height))
				self.converter()
				x=self.main.coords()[0]
				self.canvas.clear()
				self.main.delete()
				self.main=self.canvas.image(x,image=self.timage)
		if self.drag.cget('text')=='undrag':
			self.main=Draggable(self.main)
	
	def make_drag(self):
		if self.drag.cget('text')=='drag':
			self.main=Draggable(self.main)
			self.drag.config(text='undrag')
		else:
			x=self.main.coords()[0]
			self.converter()
			self.main.delete()
			self.canvas.clear()
			self.main=self.canvas.image(x,image=self.timage)
			self.drag.config(text='drag')
			
		
test=Imgr()
test.get_images()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
