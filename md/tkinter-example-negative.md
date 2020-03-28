---
title: tkinter example negative (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'negative'

Functions in program: 
* `def negative(img):`

## python negative

Python tkinter example: negative

```python
"""Negative."""


def negative(img):
    pix = img.load()
    width, height = img.size
    print(img.mode)
    for x in range(width):
        for y in range(height):
            color = pix[x, y]
            if img.mode == 'RGB':
                r, g, b = color
                new_color = (255 - r, 255 - g, 255 - b)
            elif img.mode == 'RGBA':
                r, g, b, a = color
                new_color = (255 - r, 255 - g, 255 - b, a)
            pix[x, y] = new_color
    img.save(img.filename)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
