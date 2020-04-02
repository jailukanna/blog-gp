---
title: pil example parrots (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'parrots'

Functions in program: 
* `def display(arr, front='#', back=' '):`
* `def char_to_pixels(text, path, fontsize):`

Modules used in program: 
* `import numpy as np`

## python parrots

Python pil example: parrots

```python
# https://stackoverflow.com/questions/36384353/generate-pixel-matrices-from-characters-in-string
# FONT: https://fontstruct.com/fontstructions/show/1404190/cg-pixel-4x5-1

from PIL import Image
from PIL import ImageFont
from PIL import ImageDraw
import numpy as np

def char_to_pixels(text, path, fontsize):
    font = ImageFont.truetype(path, fontsize) 
    w, h = font.getsize(text)  
    h *= 2
    image = Image.new('L', (w, h), 1)  
    draw = ImageDraw.Draw(image)
    draw.text((0, 0), text, font=font) 
    arr = np.asarray(image)
    arr = np.where(arr, 0, 1)
    arr = arr[(arr != 0).any(axis=1)]
    return arr

def display(arr, front='#', back=' '):
    result = np.where(arr, front, back)
    return [''.join(row) for row in result]

input_str = 'PYTHON'
fontsize = 5

chars = []
for c in input_str:
    arr = char_to_pixels(
    c, 
    'cg-pixel-3x5-mono.ttf',
    fontsize=fontsize)
    chars.append(display(arr, back=':shuffle_parrot:', front=':catdance:'))

print('\n'.join(''.join(row) for row in zip(*chars)))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
