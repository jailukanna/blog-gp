---
title: PIL example CompositeCentered (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'CompositeCentered'

Functions in program: 
* `def make_composite_centered(dir,frames,timeres,values):`

Modules used in program: 
* `import numpy as np`
* `import os`

## python CompositeCentered

Python PIL example: CompositeCentered

```python
import os
from PIL import Image
from PIL import ImageFont
from PIL import ImageDraw
import numpy as np

def make_composite_centered(dir,frames,timeres,values):
    filename = dir + '/../../fig_c.png'
    if os.path.isfile(filename):
        os.remove(filename)
    images = os.listdir(dir)
    images = [d for d in images if "png" in d and "composite" not in d]
    images.sort()

    images = [dir + "/" + images[i] for i in frames]
    images = map(Image.open, images)
    widths, heights = zip(*(i.size for i in images))
    max_width = max(widths)
    sum_height = sum(heights)

    new_im = Image.new('RGB', (max_width, sum_height))
    y_offset = 0
    font = ImageFont.truetype("/Library/Fonts/Arial.ttf", 60)
    for i, im in enumerate(images):

        total_length = 8.5
        percentage = (values[frames[i]]+ total_length/2 )/total_length

        crop_size = int((percentage)*widths[i])-widths[i]/2

        im1 = im.crop([0, 0, crop_size, heights[i]])
        im2 = im.crop([crop_size, 0, widths[i], heights[i]])
        ima = Image.new('RGB', (widths[i], heights[i]))
        ima.paste(im1, (widths[i]-crop_size, 0))
        ima.paste(im2, (0, 0))
        draw = ImageDraw.Draw(ima)
        draw.text((80, 50), str((frames[i] - frames[0]) * timeres) + " s", (0, 0, 0), font=font)
        new_im.paste(ima, (0, y_offset))

        y_offset += im.size[1]
    new_im.save(filename)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
