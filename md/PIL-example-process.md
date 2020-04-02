---
title: PIL example process (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'process'

Functions in program: 
* `def test():`
* `def blur():`
* `def bad():`
* `def most():`
* `def cin():`

## python process

Python PIL example: process

```python
from PIL import Image
from math import floor

from PIL import ImageFilter


def cin():
    pil_im = Image.open('test.png')
    width = pil_im.width
    height = pil_im.height
    box_width = floor(30 / 100 * width)
    box_height = floor(15 / 100 * height)

    start_left = floor(40 / 100 * width)
    start_up = floor(30 / 100 * height)
    print(start_left)
    print(start_up)
    print(box_width)
    print(box_height)
    box = (start_left, start_up, start_left + box_width, start_up + box_height)
    region = pil_im.crop(box)
    region.show()
    region.save('number.png')


def most():
    pil_im = Image.open('lena.png')
    color = sorted(pil_im.convert('RGB').getcolors())[-1:][0][1]
    result = Image.new('RGB', [pil_im.width, pil_im.height], color)

    result = Image.blend(pil_im, result, 0.8)

    result.save('result.png')


def bad():
    pil_im = Image.open('big.jpg')
    quality = 2
    pil_im.save('big_bad.jpg', 'JPEG', quality=quality)


def blur():
    pil_im = Image.open('big_2.jpg')
    result = pil_im.filter(ImageFilter.GaussianBlur(radius=100))
    result = result.filter(ImageFilter.BLUR)
    result = result.filter(ImageFilter.GaussianBlur(radius=100))
    result = result.filter(ImageFilter.BLUR)
    result.thumbnail((pil_im.width/5, pil_im.height/5), Image.LANCZOS)
    result.save('gauss.jpg')


def test():
    pil_im = Image.open('lena.png')
    print(pil_im.getextrema())


blur()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
