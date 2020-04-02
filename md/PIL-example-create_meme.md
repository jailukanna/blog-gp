---
title: PIL example create meme (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'create meme'

Functions in program: 
* `def create(source_path, target_path, name):`

Modules used in program: 
* `import PIL`

## python create meme

Python PIL example: create meme

```python
# -*- coding: utf-8 -*-

"""Create meme of a single image""" 
"""Text hardcoded in function (sorry)"""

import PIL
from PIL import ImageFont
from PIL import Image
from PIL import ImageDraw

font = ImageFont.truetype("/usr/share/fonts/TTF/LiberationSans-Bold.ttf",25)
text_left_start = 180

text_color = (117, 44, 29)
bkg_color = (214,214,214)
#bkg_color = (173, 242, 34)
#text_color = (89, 89, 89)


def create(source_path, target_path, name):

    img=Image.new("RGBA", (600,211), bkg_color)

    draw = ImageDraw.Draw(img)
    draw.text((text_left_start, 50), u'Здраво, јас сум ',text_color, font=font)
    draw.text((text_left_start, 80),  name , text_color, font=font)
    draw.text((text_left_start, 110), u'и гласав „ЗА“', text_color, font=font)
    draw.text((text_left_start, 140), u'цензура на Интернет.', text_color, font=font)

    draw = ImageDraw.Draw(img)

    icon = Image.open(source_path + '/' + name + ".jpg")
    x, y = icon.size

    img.paste(icon, (10,10,x+10,y+10))

    img.save(target_path + "/" + name + ".png")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
