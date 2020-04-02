---
title: pil example Gui2 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'Gui2'


Modules used in program: 
* `import sound`

## python Gui2

Python pil example: Gui2

```python
from scene import *
from random import random
import sound
from PIL import Image, ImageDraw

# Base class for controls
class Window (Layer):
	# Create a default window
	def __init__(self,p,bounds):
				Layer.__init__(self, bounds)
			
				# Add ourself to parent layer list
				if p: p.add_layer(self)		
				
				self.background=Color(1,1,1)
		
	# Skeleton functions to be overriden		
	def touch_began(self,touch): pass
	def touch_moved(self,touch): pass
	def touch_ended(self,touch): pass

#-------------------------------------------------
class Button (Window):
	def __init__(self,p,b):
		Window.__init__(self,p,b)
		
		# Default to a red border of thickness 1.0
		self.stroke = Color(1,0,0)
		self.stroke_weight=1
		self.image = 'Snake'
	
	def touch_began(self,touch):
		new_color = Color(random(), random(), random())
		self.animate('background', new_color, 1.0)
		sound.play_effect('Crashing')
#-------------------------------------------------

# Test Button class with rounded corners using alpha blended mask image
class Button2 (Window):
	def __init__(self,p,b):
		Window.__init__(self,p,b)

		# Create a 300 wide by 120 blue button
		grad = Image.new('RGBA', (300, 120))
		draw = ImageDraw.Draw(grad)
		draw.rectangle((0,0,300,120),fill='DeepSkyBlue')

		# Round the rectangle corners
		mask = Image.open('_button-mask.png').convert('RGBA').split()[3]
		grad.putalpha(mask)
		
		self.image=load_pil_image(grad)
		
		self.stroke=Color(0,0,0)
		self.stroke_weight=1
	
	def touch_began(self,touch):
		sound.play_effect('Crashing')

#-------------------------------------------------

# Test Button class with rounded corners using pieslices
class Button3 (Window):
	def __init__(self,p,where=(0,0),size=(200,40)):
		(x,y) = where
		(w,h) = size
		Window.__init__(self,p,Rect(x,y,w,h))

		# Create an image to old the button image
		g = Image.new('RGBA', (w,h))
		draw = ImageDraw.Draw(g)
		
		# Basic fill is even light grey
		draw.rectangle((0,0,w,h),fill='DarkGrey')
		
		# Round the rectangle corners and show the image
		self.image=load_pil_image(self.add_corners(g,8))		
		
		# Or just show the unrounded image
#		self.image = load_pil_image(g)
		

	# Function to build/apply a mask to round off the corners of a rectangular image
	def add_corners(self, im, rad):
		# Start with an image big enough to hold a circle of radius rad
		circle = Image.new('L', (rad * 2, rad * 2), 0)
		draw = ImageDraw.Draw(circle)
		
		# Draw an ellipse (really a circle) 2*radius in diameter
		draw.ellipse((0, 0, rad * 2, rad * 2), fill=255)
		
		# Create an image the same size as the rectangle to be rounded
		alpha = Image.new('L', im.size, 255)
		w, h = im.size
		
		# Top left
		alpha.paste(circle.crop((0,0,rad,rad)), (0,0))
		
		# Bottom left
		alpha.paste(circle.crop((0, rad, rad, rad * 2)), (0, h - rad))
		
		# Top right
		alpha.paste(circle.crop((rad, 0, rad * 2, rad)), (w - rad, 0))
		
		# Bottom right
		alpha.paste(circle.crop((rad, rad, rad * 2, rad * 2)), (w - rad, h - rad))
		
		# Apply the mask then return the image
		im.putalpha(alpha)
		return im
	
	def touch_began(self,touch):
		sound.play_effect('Crashing')
		
#------------------------------------------------

# A window (a Layer) containing buttons
class ButtonBar (Window):
	def __init__(self,p,b,n):
		Window.__init__(self,p,b)
		
		# Parent window
		b = self.frame
		x = b.x
		
		for i in range(n):
			Button(self,Rect(x,b.y,b.w,b.h))
			x=x+128
		
#-------------------------------------------------

# A window (a Layer) containing Text
class Text (Window):
	def __init__(self,p,o,t,f,s):
		Window.__init__(self,p,Rect(o.x,o.y,0,0))
		self.tint = Color(0,0,0)
		self.text_img, ims = render_text(t,font_name=f, font_size=s)
		self.frame=Rect(o.x,o.y,ims.w,ims.h)

		self.image = self.text_img
		
		# Default to a red border of thickness 1.0
		self.stroke = Color(1,0,0)
		self.stroke_weight=1


	def touch_began(self,touch):
		sound.play_effect('Beep')
#-------------------------------------------------

class MyApp (Scene):
	
	# This runs before any frames or layers are drawn
	def setup(self):
	
		# This is our background canvas (whole display)
		p = self.root_layer = Layer(self.bounds)
		
		center = self.bounds.center()
		
		# Create 2 primitive buttons as children of root layer
		Button(p,Rect(center.x + 80, center.y + 80, 128, 128))
		Button3(p,where=Point(center.x - 80, center.y - 80))
		
		# Now try a button bar
		ButtonBar (p,Rect(10,10,128,128),5)
		
		# And a label
		Text (p,Point(10,200),'Woof', 'Futura', 100)
		
		# And another label
		Text (p,Point(10,400),'2nd text string', 'Futura', 12)
		      
	def draw(self):
		# White background - basically display.clear() before redraw
		background(1, 1, 1)
		
		self.root_layer.update(self.dt)
		self.root_layer.draw()
	
	def touch_began(self, touch):
		l=touch.layer
		if l is Window: l.touch_began(touch)
	
	def touch_moved(self, touch):
		l=touch.layer
		if l is Window: l.touch_moved(touch)
	
	def touch_ended(self, touch):
		l=touch.layer
		if l is Window: l.touch_ended(touch)
		
run(MyApp())


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
