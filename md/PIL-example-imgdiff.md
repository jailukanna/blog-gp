---
title: PIL example imgdiff (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'imgdiff'

Functions in program: 
* `def rmsdiff(im1, im2):`

Modules used in program: 
* `import math, operator`
* `import PIL`
* `import sys`

## python imgdiff

Python PIL example: imgdiff

```python
import sys
import PIL
from PIL import ImageChops
from functools import reduce
import math, operator

def rmsdiff(im1, im2):
    "Calculate the root-mean-square difference between two images"

    h = ImageChops.difference(im1, im2).histogram()

    print(h)
    # calculate rms
    return math.sqrt(reduce(operator.add,
        map(lambda h, i: h*(i**2), h, range(256))
    ) / (float(im1.size[0]) * im1.size[1]))


if __name__ == '__main__':
    a= PIL.Image.open(sys.argv[1])
    b= PIL.Image.open(sys.argv[2])
    print((rmsdiff(a,b)))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
