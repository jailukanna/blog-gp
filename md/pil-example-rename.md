---
title: pil example rename (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'rename'


Modules used in program: 
* `import PIL`
* `import datetime`
* `import glob`
* `import os`

## python rename

Python pil example: rename

```python
import os
import glob
import datetime
import PIL
from PIL import ImageFont
from PIL import Image
from PIL import ImageDraw

i = 0

for f in glob.glob('*.png'):
    i+=1
    newname = '%.4d.png' % i

    basename, _ = os.path.splitext(f)
    year = int(basename[0:4])
    month = int(basename[4:6])
    day = int(basename[6:8])
    d = datetime.date(year, month, day)
    text = d.strftime("%h %d, %Y")
    font = ImageFont.truetype("/System/Library/Fonts/LucidaGrande.ttc",55)
    tcolor = (0,0,0)
    text_pos = (140,100)

    img = Image.open(f)
    draw = ImageDraw.Draw(img)
    draw.text(text_pos, text, fill=tcolor, font=font)
    del draw

    img.save(newname)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
