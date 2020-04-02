---
title: PIL example create consecutive image (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'create consecutive image'

Functions in program: 
* `def create_consecutive_image(num):`

## python create consecutive image

Python PIL example: create consecutive image

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

# pip install pillow

from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont

WIDTH = 480
HEIGHT = 320

TRUE_TYPE_FONT = '/Library/Fonts/Osaka.ttf'
FONT_SIZE = 64


def create_consecutive_image(num):
    size = len(str(num))
    filename_base = '%0' + str(size) + 'd.jpg'
    for i in range(1, num + 1):
        filename = filename_base % i

        background_color = (255, 204, 204)
        font_color = (50, 50, 50)

        img = Image.new('RGB', (WIDTH, HEIGHT), background_color)

        font = ImageFont.truetype(TRUE_TYPE_FONT, FONT_SIZE, encoding='utf-8')

        draw = ImageDraw.Draw(img)
        draw.text((50, 50), filename, font=font, fill=font_color)

        img.save(filename)


if __name__ == '__main__':
    create_consecutive_image(10)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
