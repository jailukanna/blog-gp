---
title: PIL example ksnctf22 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'ksnctf22'


Modules used in program: 
* `import numpy`

## python ksnctf22

Python PIL example: ksnctf22

```python
from PIL import Image
import numpy

const = 31

data = [[] for i in range(const)]
for i in range(const):
    s = str(input())
    for j in range(len(s)):
        if ord('A') <= ord(s[j]) and ord(s[j]) <= ord('Z'):
            data[i].append(1)
        else:
            data[i].append(0)

coeff = 10
screen = (const * coeff, const * coeff)
bgColor=(0xff, 0xff, 0xff)
filename = "ksnctf22.png"
black = [0, 0, 0]

img = Image.new('RGB', screen, bgColor)
maxcol, maxrow = img.size
imgArray = numpy.asarray(img)
imgArray.flags.writeable = True

for i in range(const):
    for j in range(const):
        if data[i][j] == 1:
            imgArray[(i * coeff) : (i * coeff) + coeff, (j * coeff) : (j * coeff) + coeff] = black


pilOUT = Image.fromarray(numpy.uint8(imgArray))
pilOUT.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
