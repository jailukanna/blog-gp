---
title: pil example watermark (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'watermark'

Functions in program: 
* `def test():`
* `def water(img, watermark):`

Modules used in program: 
* `import os, sys`

## python watermark

Python pil example: watermark

```python
"""
This is a python script used to watermark your Image
It use Python Imaging Library
Dependence:
    Here is a link show how to install PIL correctly: http://askubuntu.com/questions/156484/how-do-i-install-python-imaging-library-pil
It works at Linux 3.2.0-4-686-pae #1 SMP Debian 3.2.46-1 i686 GNU/Linux
Free to use it.
How to use it:
    python watermark.py demo.jpg  # it will save the watermarked image as waterdemo.jpg at current directory. 
"""

from PIL import Image, ImageEnhance
import os, sys
 
def water(img, watermark):
    mark = Image.open(watermark)
    try:
        im = Image.open(img)
        if im.mode != 'RGBA':
            im = im.convert('RGBA')
        layer = Image.new('RGBA', im.size, (0,0,0,0))
        position = (im.size[0]-mark.size[0], im.size[1]-mark.size[1])
        layer.paste(mark, position)
        Image.composite(layer, im, layer).save("water" + img)
    except Exception, (msg):
        print(msg)

def test():
    water(sys.argv[1], "water.png") 
          
if __name__ == '__main__':
    test()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
