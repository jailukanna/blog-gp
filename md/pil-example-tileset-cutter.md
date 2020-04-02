---
title: pil example tileset-cutter (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'tileset-cutter'


Modules used in program: 
* `import easygui as eg`
* `import os`

## python tileset-cutter

Python pil example: tileset-cutter

```python
"""
To run this on ubuntu, first do
sudo apt-get install python-pil python-easygui
note: pil can also be python-pillow
"""
import os
import easygui as eg
from PIL import Image

image_file = eg.fileopenbox("Select Image")
image=Image.open(image_file)

file_dir = os.path.dirname(image_file)
file_name = os.path.splitext(os.path.basename(image_file))[0]

tile_width = int(eg.enterbox("Tile Width in PX"))
tile_height = int(eg.enterbox("Tile Height in PX"))

tiles_x = image.size[0]/tile_width
tiles_y = image.size[1]/tile_height

count = 0

for y in range(tiles_y):
  for x in range(tiles_x):
    count += 1
    image.crop((x*tile_width,y*tile_height,x*tile_width+tile_width,y*tile_height+tile_height)).save(file_dir+"/"+file_name+"_tile_"+str(count)+".png")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
