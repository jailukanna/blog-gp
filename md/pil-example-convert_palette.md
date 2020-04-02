---
title: pil example convert palette (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'convert palette'

Functions in program: 
* `def nearest(pixel_color, mem={}):`

## python convert palette

Python pil example: convert palette

```python
def nearest(pixel_color, mem={}):
    if pixel_color in mem:
        return mem[pixel_color]
    n = min(floss_palette, key=lambda fc:delta_e_cie2000(pixel_color, fc))
    mem[pixel_color] = n
    return mem[pixel_color]

result = [nearest(pixel_color) for pixel_color in image]

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
