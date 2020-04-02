---
title: pil example wallpaper (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'wallpaper'


Modules used in program: 
* `import PIL`

## python wallpaper

Python pil example: wallpaper

```python
import PIL
from PIL import ImageFont
from PIL import Image
from PIL import ImageDraw

# font = ImageFont.truetype("Arial-Bold.ttf",14)
font = ImageFont.truetype("ArialUnicode.ttf",34)
img=Image.new("RGBA", (500,250),(255,255,255))
draw = ImageDraw.Draw(img)
draw.text((0, 0),"This is a test",(0,0,0) ,font=font)
draw = ImageDraw.Draw(img)
img.save("a_test.png")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
