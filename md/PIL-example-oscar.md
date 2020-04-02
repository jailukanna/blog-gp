---
title: PIL example oscar (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'oscar'

Functions in program: 
* `def draw_text(draw, text, position, font_size=25, font_color=(255,255,255), font_family="HelveticaNeue"):`
* `def draw_elipse(draw, position, r, fill=(255,255,255)):`
* `def path_to(filename):`

Modules used in program: 
* `import os`

## python oscar

Python PIL example: oscar

```python
import os
from PIL import Image
from PIL import ImageFont
from PIL import ImageDraw 

APPLICATION_DIR = os.path.abspath(os.path.dirname(__file__))

def path_to(filename):
	return os.path.join(APPLICATION_DIR, filename)

def draw_elipse(draw, position, r, fill=(255,255,255)):
	draw.ellipse((position[0]-r, position[1]-r, position[0]+r, position[1]+r), fill=fill)

def draw_text(draw, text, position, font_size=25, font_color=(255,255,255), font_family="HelveticaNeue"):
	font_file = path_to("fonts/%s.ttf" % font_family)
	font = ImageFont.truetype(font_file, font_size)
	w, h = draw.textsize(text, font=font)
	draw.text(((position[0]-w/2),(position[1]-h/2)), text, font_color, font=font)


bg = Image.open(path_to("poster.jpg"))
# fg = Image.open(path_to("oscar.png"))

draw = ImageDraw.Draw(bg)
draw_elipse(draw, (bg.size[0]/2, -10), 150, (212, 183, 62))
draw_elipse(draw, (bg.size[0]/2, -10), 110, (234, 201, 68))
draw_text(draw, "NOMINACIONES", (bg.size[0]/2, 15), font_color=(200, 200, 200))
draw_text(draw, "NOMINACIONES", (bg.size[0]/2-1, 15-1), font_color=(67, 72, 82))
draw_text(draw, "7", (bg.size[0]/2, 50), font_color=(67, 72, 82), font_size=68)


# bg.paste(fg, ((bg.size[0]-fg.size[0], bg.size[1]-fg.size[1])), fg)
bg.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
