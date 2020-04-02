---
title: pil example disco dancer (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'disco dancer'


## python disco dancer

Python pil example: disco dancer

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Author: john
# @Date:   2017-02-15 15:15:24
# @Last Modified by:   john
# @Last Modified time: 2017-02-16 18:52:57

from PIL import Image
from PIL import ImageDraw
from subprocess import call
from random import randint

white = (255,255,255)
green = (0,255,0)
black = (0, 0, 0)

size = width, height = 800, 800

flag = "USCGA{PIL_to_process_images}"

box_size = 32
s = box_size/2

center_x, center_y = width/2, height/2

im = Image.new('RGB', size, white )

draw = ImageDraw.Draw(im)


px, py = center_x, center_y

path = "RRRRRRDDDDDDDLLLLLLLLLLUUUUU"

path = path.replace('U', 'A')
path = path.replace('D', 'B')
path = path.replace('R', 'C')
path = path.replace('L', 'D')

print(path)

for x in range( 0, width+box_size, box_size ):
	for y in range( 0, height+box_size, box_size ):
		color = randint(41, 122)
		# color = 0
		draw.rectangle(  [(x - box_size, y - box_size), (x + box_size, y + box_size)],
					  ( color, color, color),
					  ( color, color, color)),

draw.rectangle(  [(center_x - s, center_y - s), (center_x + s, center_y + s)],
				# [(center_x - box_size, center_y - box_size), (center_x + box_size, center_y + box_size)],
				  green, green)

for i, point in enumerate(path):

	if point == "A": py -= box_size
	if point == "B": py += box_size
	if point == "C": px += box_size
	if point == "D": px -= box_size

	draw.rectangle(  [(px - s, py - s), (px + s, py + s)],
				  ( ord(flag[i]), ord(flag[i]), ord(flag[i]) ),
				  ( ord(flag[i]), ord(flag[i]), ord(flag[i]) ))

path = path.replace('A', '^[[A[')
path = path.replace('B', '^[[B[')
path = path.replace('C', '^[[C[')
path = path.replace('D', '^[[D[')


im.show()
im.save('disco_dancer.png')
call(str('exiftool disco_dancer.png -comment="'+path+'"').split())


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
