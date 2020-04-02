---
title: PIL example fastpic (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'fastpic'

Functions in program: 
* `def main():`
* `def generate(first, second):`

Modules used in program: 
* `import numpy`

## python fastpic

Python PIL example: fastpic

```python
# Adam Anderson

import numpy
from PIL import Image

# Colors
WHITE = 0xFFFFFFFF
BLACK = 0xFF000000
RED   = 0xFF0000FF

def generate(first, second):
    # Image Data
    w,h = 200,200
    img = numpy.empty((w,h),numpy.uint32)
    img.shape=h,w

    # SET COLORS ON IMAGE
    # img[startrow:finishrow+1,starx:endx]
    img[0:50, 0:200] = first
    img[51:100, 0:200] = second
    img[101:150, 0:200] = first
    img[151:200, 0:200] = second

    pilImage = Image.frombuffer('RGBA',(w,h),img,'raw','RGBA',0,1)    
    return pilImage

def main():
    png = generate(WHITE, BLACK)
    png.save('my.png')
    
if __name__ == "__main__":
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
