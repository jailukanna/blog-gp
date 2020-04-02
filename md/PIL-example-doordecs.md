---
title: PIL example doordecs (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'doordecs'


Modules used in program: 
* `import PIL`

## python doordecs

Python PIL example: doordecs

```python
import PIL
from PIL import ImageFont
from PIL import Image
from PIL import ImageDraw
font = ImageFont.truetype("/usr/share/fonts/dejavu/DejaVuSans.ttf", 75)

namelist = [
    # names go here
]


splitnames = []

for i in range(0, len(namelist), 4):
    sublist = []
    for j in range(i, i + 4):
        try:
            sublist.append(namelist[j])
        except IndexError:
            sublist.append("")
    splitnames.append(sublist)

img_num = 1
for n1, n2, n3, n4 in splitnames:
    img = Image.open('dec.jpg')
    draw = ImageDraw.Draw(img)
    draw.text((100, 150), n1, (0, 0, 0), font=font)
    draw.text((730, 150), n2, (0, 0, 0), font=font)
    draw.text((100, 550), n3, (0, 0, 0), font=font)
    draw.text((730, 550), n4, (0, 0, 0), font=font)
    draw = ImageDraw.Draw(img)
    img.save("doordec_%s.png" % img_num)
    img_num += 1

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
